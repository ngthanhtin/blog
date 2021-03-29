---
layout: post
title: Bilinear (Trilinear) Attention
subtitle: Attention
date : "2021-03-29"
tags: [bilinear, vqa, attention]
comments: true
cover: "/blog/img/bilinear_att/bilinear.jpg"
---
Hi các bạn, ở bài post này mình tiếp tục chia sẽ một kĩ thuật Attention dùng trong bài toán <b>Visual Question Answering (VQA)</b> đó là <b>Bi-Attention (Tri-Attention)</b>, ở bài trước mình có đề cập tới một kĩ thuật khác gọi là <b>[Stacked Attention](https://ngthanhtin.github.io/blog/2021-02-01-stacked-attention-network/)</b><br/>
Link paper: [Bilinear Attention](https://arxiv.org/abs/1805.07932)<br/>
Link code: [Code](https://github.com/jnhwkim/ban-vqa)<br/>

## 1. Introduction
Đầu tiên, chúng ta cần hiểu VQA là gì, nếu bạn chưa biết thì có thể xem bài viết trước của mình để hiểu nó, [link](https://ngthanhtin.github.io/blog/2021-02-01-stacked-attention-network). <br/>
Nguyên lý của bài toán này đó là làm sao có thể kết hợp 2 feature vision và text lại với nhau thành một vector chung, sau đó đưa vector này sang một classifier để tìm ra answer, và có nhiều phương pháp hiện nay sử dụng để kết hợp 2 feature đó lại với nhau. Về cơ bản, chúng ta có thể sử dụng phương pháp concatenate để nối 2 feature vector, hoặc dùng element-wise product (hoặc là Hardarmard product) để 2 feature tương tác với nhau, hoặc là dùng bilinear pooling để 2 vector có thể tương tác với nhau nhiều hơn vì vậy cung cấp một attended vector tốt hơn. <br/>
<p align="center">
<img src="/blog/img/bilinear_att/hardarmard.png">
</p>
<div style="text-align: center">Element-wise product (Hardarmard).</div>

<p align="center">
<img src="/blog/img/bilinear_att/outer_product.png">
</p>
<div style="text-align: center">Outer product, core component của bilinear pooling.</div>

Khi dùng concatenation, đây là một phương pháp khá nice, dễ sử dụng, đơn giản bạn chỉ cần việc nối 2 vector lại và đưa vector đó qua thành phần tiếp theo (là một classifier để phân loại). Tuy nhiên, cách này ko làm cho 2 vector vision và text tương tác với nhau hiệu quả, vì thế người ta mới sử dụng những phương pháp như element-wise hay outer-product. 
Điểm mạnh của element-wise là 2 vector tương tác với nhau rất tốt, bạn có thể nhìn vào cách tính là sẽ thấy được, tuy nhiên nó có 2 yếu điểm sau: (1) Nếu 2 vector quá lớn, dẫn tới việc tính toán cũng sẽ vô cùng lớn, làm chậm quá trình trainning, (2) nó yêu cầu 2 vector phải có cùng số chiều. <br/>
Còn về outer product hay bilinear pooling, nó cho phép tạo ra một vector attention khi 2 vector vision và text không cùng chiều. Và để làm được điều đó đơn giản chỉ cần chèn vào ở giữa 2 vector này một thằng trung gian:<br/>
<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=z_{i} = x^{T}W_{i}y">
</p>
<div style="text-align: center">Với x là vector của vision, y là vector của text.</div>
Và bởi vì phương pháp này dựa trên outer product nên nó cũng có thể tạo ra được một vector có sự tương tác tốt hơn giữa 2 vector vision và text, nhìn vào cách tính của outer product bạn có thể thấy được mỗi giá trị của vector này được nhân với tất cả giá trị của vector kia, nên kết quả là vector fusion có được interaction rất tốt.<br/>
Ví dụ, x là (40x60), text là (100,) thì W là (40x100). <br/>

### 2. Low-rank bilinear pooling
Khi nhìn vào Bilinear Pooling chúng ta có thể thấy, ứng với mỗi z_i, ta cần phải học một ma trận W_i tương ứng, việc này dẫn đến computational cost rất cao. Để giải quyết vấn đề này, ta sử dụng một kĩ thuật trong đại số tuyến tính là <b>Low-rank factorization</b>, nghĩa là:<br/>
<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=z_{i} = x^{T}W_{i}y \approx x^{T}U_{i}V_{i}{T}y = \sum_{d=1}^{k}x^{T}u_{d}v_{d}{T}y = \mathbb{1}^{T}(U_{i}^{T}x \circ V_{i}^{T}y)">
</p>
<img src="https://render.githubusercontent.com/render/math?math=\circ"> là kí hiệu của Hadarmard product <img src="https://render.githubusercontent.com/render/math?math=\mathbb{1}"> là vector bào gồm tất cả value là 1. <br/>
Ta có thể thấy, k chính là một chiều ẩn của factorized matrix Ui và Vi. Ta có thể dùng thay thế một pooling matrix để mô tả quá trình này:<br/>
<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=z = P^{T}(U^{T}x \circ V^{T}y)">
</p>
Việc sử dụng Pooling ở đây khiến cho số lượng parameter giảm đi đáng kể. Vì vậy low-rank bilinear vẫn giữ được tính chất của bilinear pooling nhưng đồng thời làm giảm số lượng parameter rất nhiều nhờ vào kĩ thuật fatorized matrix.

### 3. Bilinear Attention

### 4. Trilinear Attention

## 5. Tham khảo
Fukui, Akira, et al. "Multimodal compact bilinear pooling for visual question answering and visual grounding." arXiv preprint arXiv:1606.01847 (2016).<br/>
Kim, Jin-Hwa, et al. "Hadamard product for low-rank bilinear pooling." arXiv preprint arXiv:1610.04325 (2016).<br/>
https://medium.com/@nithinraok_/visual-question-answering-attention-and-fusion-based-approaches-ebef62fa55aa <br/>

<div style="text-align: right"> (Tín Nguyễn) </div>