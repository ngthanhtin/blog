---
title : "Tìm hiểu Categorical Cross-Entropy Loss, Binary Cross-Entropy Loss, Softmax Loss, Logistic Loss, Focal Loss, you name it"
cover : "/blog/img/tim_hieu_loss_function/loss.png"
date : "2021-02-21"
tags : 
  - "loss function"
categories : "ML Technique"

---

Hi, xin chào các bạn, hôm này mình sẽ giải thích một số  hàm loss cho các bạn như <b>Cross entropy loss</b>, <b>binary cross-entropy loss</b>, etc<br/>
Mình làm bài viết này bởi vì tên của các hàm loss này thường rất dễ bị nhầm lẫn, cộng với việc mỗi framework lại có những cái tên na ná nhau, vì vậy mình sẽ giúp các bạn phân biệt chúng.

Nội dung chính sẽ bao gồm các phần sau: <br/>

<a href="#1. Task">1. Task</a><br/>

<a href="#2. Cross-Entropy Loss">2. Cross-Entropy Loss</a>

<a href="#3. Categorical Cross-Entropy Loss">3. Categorical Cross-Entropy Loss</a>

<a href="#4. Binary Cross-Entropy Loss">4. Binary Cross-Entropy Loss</a>

<a href="#5. Tham khảo">6. Tham khảo</a>

<section id="1. Task">
<b>1. Task</b>
</section>
<b>1.1 Multi-Class Classification</b><br/>
Đây là tác vụ phân loại một mẫu vào một class nào đó. Ví dụ có 100 class, và phải phân loại một mẫu vào 1 trong 100 class này.

<b>1.2 Multi-Label Classification</b><br/>
Đây là tác vụ mà một mẫu có thể thuộc về nhiều class. Ví dụ có 100 class, và một mẫu nào đó có thể thuộc class 1 và 3 và 5, etc.

Ta có thể phân biệt 2 tác vụ này qua hình ảnh sau:
<p align="center">
  <img src="/blog/img/tim_hieu_loss_function/task.png">
</p>

<section id="2. Cross-Entropy Loss">
<b>2. Cross-Entropy Loss</b>
</section>
Cross-Entropy Loss được dùng trong task vụ thứ 1, công thức của nó như sau: <br/>
<p align="center">
<img src="https://render.githubusercontent.com/render/math?math=CE=-\sum_{i}^{C}t_{i}log(s_{i})">
</p>
Where t_i và s_i là ground truth và score của mỗi class i. Trước khi tính toán CE loss, ta thường đưa qua một activation function như sigmoid, softmax. <br/>
Trong trường hợp chỉ có 2 class, hàm loss có thể được viết như sau và gọi là Binary Cross Entropy: <br/>
<p align="center">
<img src="https://render.githubusercontent.com/render/math?math=CE=-\sum_{i}^{C=2}t_{i}log(s_{i}) = -t_{1}log(s_{1})-(1-t_{1})log(1-s_{1})">
</p>
Logistic Loss và Multinomial Logistic Loss là những cái tên khác của Cross-Entropy loss.

<section id="3. Categorical Cross-Entropy Loss">
<b>3. Categorical Cross-Entropy Loss</b>
</section>
Còn được gọi là Softmax cross-entropy, nó chỉ đơn giản là softmax activation theo sau là cross-entropy loss, và hàm loss này cũng được dùng cho tác vụ thứ 1.<br/>
<p align="center">
  <img src="/blog/img/tim_hieu_loss_function/softmax_loss.png">
</p>
Với <img src="https://render.githubusercontent.com/render/math?math=f(s)"> là hàm softmax.
<section id="4. Binary Cross-Entropy Loss">

<b>4. Binary Cross-Entropy Loss</b>
</section>
Còn được gọi là Sigmoid cross-entropy, nó chỉ đơn giản là sigmoid activation theo sau là cross-entropy loss, và hàm loss này cũng được dùng cho tác vụ thứ 2.<br/>
Không như Softmax loss mỗi output score sẽ không phụ thuộc với nhau, nghĩa là loss tính từ CNN output vector không bị ảnh hưởng bởi các giá trị vector khác. Đó là lí do tại sao hàm loss này được sử dụng cho tác vụ thứ 2.<br/>
Công thức cho hàm loss này như sau: <br/>
<p align="center">
<img src="https://render.githubusercontent.com/render/math?math=CE=-\sum_{i}^{C=2}t_{i}log(s_{i}) = -t_{1}log(f(s_{1}))-(1-t_{1})log(1-f(s_{1}))">
</p>
Với <img src="https://render.githubusercontent.com/render/math?math=f(s_{i})"> là hàm sigmoid.
<section id="5. Tham khảo">
<b>6. Tham khảo</b>
</section>
https://gombru.github.io/2018/05/23/cross_entropy_loss/ <br/>
https://towardsdatascience.com/understanding-binary-cross-entropy-log-loss-a-visual-explanation-a3ac6025181a<br/>
<div style="text-align: right"> (Tín Nguyễn) </div>
