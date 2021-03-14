---
title : "Giới thiệu về Optmization"
cover : "/blog/img/math_optimization/introduction.png"
date : "2021-03-14"
tags : 
  - "math"
  - "optmization"

---

Hi, xin chào các bạn, hôm này mình sẽ giới thiệu cho các bạn về toán <b>Optmization</b>. Nói về Deep Learning thì không thể thiếu được Optimization, chắc hẳn ai làm DL đều đã xài qua những cái như SGD, Adam, Adagrad, KAFC, etc. Tác dụng của optimization đó là giúp tìm ra local minima của model, khi đó ta có thể nói model đã converge.

Tuy nhiên, để hiểu rõ cách thức hoạt động của optimization, chúng ta phải hiểu nó là gì, và nó áp dụng như thế nào? Và ở bài này, mình sẽ giới thiệu về bài toán optimization và ôn lại một số kiến thức cơ bản về toán matrix, toán calculus. Let's go.

Nội dung chính sẽ bao gồm các phần sau: <br/>

1. Giới thiệu bài toàn Optimization ?
2. Constraints ?
3. Critical Points ?
4. Điều kiện để là một Local Minima ?
5. Bàn luận.
5. Tham khảo.



### 1. Giới thiệu bài toàn Optimization ?

Bài toán Visual Question Answering (VQA) là một trong những bài toán có sự kết hợp giữa Vision và Language. Đây là bài toán trả lời một câu hỏi dựa vào hình ảnh, mô hình vì thế phải học được sự liên hệ giữa hình ảnh và ngôn ngữ, từ đó đưa ra được câu trả lời phù hợp.
<p align="center">
  <img src="https://github.com/ngthanhtin/ngthanhtin.github.io/blob/master/_data/math_optimization/introduction.png?raw=true">
</p>

### 2. Constraints ?


### 3. Critical Points ?
Bài báo đề xuất phương pháp Stacked Attention, đây là một phương pháp cho phép diễn dịch hình ảnh theo nhiều bước. Ví dụ cho câu hỏi và hình ảnh sau:<br/>
Câu hỏi: <b> What are sitting in the basket on a bicycle </b><br/>
Hình ảnh:
<p align="center">
  <img src="https://github.com/ngthanhtin/ngthanhtin.github.io/blob/master/_data/vqa/attention.png?raw=true">
</p>
Và ta sử dụng 2 lớp attention, thì lớp attention đầu tiên sẽ định vị những đối tượng như <b>basket, bicycle</b> sau đó ở những lớp attention tiếp theo sẽ dần dần loại bỏ những đối tượng không liên quan và cho ra đối tượng cần thiết.<br/>
<b>3.1 Image Model</b><br/>
Ở bài báo này, họ đã dùng mô hình VGG Net để lấy feature representation của image.<br/>
<p align="center">
  <img src="https://github.com/ngthanhtin/ngthanhtin.github.io/blob/master/_data/vqa/vgg.png?raw=true">
</p>
Đầu tiên, image <img src="https://render.githubusercontent.com/render/math?math=I"> sẽ được rescale thành 448x448, sau đó đưa qua VGG Net, tuy nhiên họ chỉ lấy feature <img src="https://render.githubusercontent.com/render/math?math=f_{I}"> của lớp pooling cuối cùng cái mà họ cho rằng giữ lại được nhiều thông tin của image hơn là lớp cuối cùng của mạng VGG.<br/>
<p align="center">
<img src="https://render.githubusercontent.com/render/math?math=f_{I} = CNN_{vgg}(I)">
</p>
Sau đó, họ biến đổi <img src="https://render.githubusercontent.com/render/math?math=f_{I}"> thành <img src="https://render.githubusercontent.com/render/math?math=v_{I}"> sao cho có cùng dimension như question vector <b>(3.2)</b><br/>
<p align="center">
<img src="https://render.githubusercontent.com/render/math?math=v_{I} = tanh(W_{I}f_{I}%2Bb_{I})">
</p>
<b>3.2 Question Model</b><br/>
Đối với text presentation, ở bài báo họ sử dụng 2 cách: <b>(1) dùng lstm và sử dụng hidden để làm vector representation cho text</b> và <b>(2) sử dụng unigram để lấy vector presentation cho từng word, sau đó concatenate các vector này lại tạo thành vector representation cho text</b><br/>
Ở post này mình chỉ nói qua phương pháp (1).<br/>
<p align="center">
  <img src="https://github.com/ngthanhtin/ngthanhtin.github.io/blob/master/_data/vqa/lstm.png?raw=true">
</p>

<p align="center">
<img src="https://render.githubusercontent.com/render/math?math=x_{t} = W_{e}q_{t}, t \in {1,2,...,T}">
</p>
<p align="center">
<img src="https://render.githubusercontent.com/render/math?math=h_{t} = LSTM(x_{t}), t \in {1,2,...,T}">
</p>
Cuối cùng, vector representation của question sẽ là hidden vector ở layer cuối cùng của LSTM, <img src="https://render.githubusercontent.com/render/math?math=v_{Q} = h_{T}"><br/>
<b>3.3 Stacked Attention Network</b><br/>
Sau khi đã có 2 vector representation cho image <img src="https://render.githubusercontent.com/render/math?math=v_{I}"> và text <img src="https://render.githubusercontent.com/render/math?math=v_{Q}">, ta sẽ đưa 2 thứ này qua mạng <b>Stacked Attention (SAN)</b> để dự đoán câu trả lời.<br/> 
Mô hình SAN được mô tả như sau: <br/>
<p align="center">
<img src="https://github.com/ngthanhtin/ngthanhtin.github.io/blob/master/_data/vqa/san.png?raw=true">
</p>
Bước đầu tiên, mô hình sẽ đưa <img src="https://render.githubusercontent.com/render/math?math=v_{I}"> và <img src="https://render.githubusercontent.com/render/math?math=v_{Q}"> qua một lớp perceptron, sau đó tạo ra <b>attention distribution</b> bằng một lớp softmax: <br/>
<p align="center">
<img src="https://render.githubusercontent.com/render/math?math=h_{A} = tanh(W_{I,A}v_{I} \bigoplus(W_{Q,A}v_{Q} %2B b_{A}))"><br/>
<img src="https://render.githubusercontent.com/render/math?math=p_{I} = softmax(W_{p}h_{A} %2B b_{p})">
</p>
Sau khi có <b>attention distribution</b>, ta lấy weighted sum của distribution và image representation theo công thức: <br/>
<p align="center">
<img src="https://render.githubusercontent.com/render/math?math=\tilde{v}_{I} = \sum_{i}p_{i}v_{i}">
</p>
Cuối cùng, tính ra query vector dùng để encode thông tin của image và question, sau đó dùng nó để tìm ra câu trả lời: <br/>
<p align="center">
<img src="https://render.githubusercontent.com/render/math?math=u = \tilde{v}_{I} %2B v_{Q}">
</p>

Tuy nhiên, mô hình SAN sẽ lặp lại các bước trên để đưa ra một query đúng hơn. Mỗi lần lặp lại là một lớp attention, vì vậy, đối với lớp attention thứ k, chúng ta sẽ tính như sau: <br/>
<p align="center">
<img src="https://render.githubusercontent.com/render/math?math=h^{k}_{A} = tanh(W^{k}_{I,A}v_{I} \bigoplus(W^{k}_{Q,A}u^{k-1} %2B b^{k}_{A}))"><br/>
<img src="https://render.githubusercontent.com/render/math?math=p^{k}_{I} = softmax(W^{k}_{p}h^{k}_{A} %2B b^{k}_{p})">
</p>
Tương tự với weighted sum và query vector:<br/>
<p align="center">
<img src="https://render.githubusercontent.com/render/math?math=\tilde{v}^{k}_{I} = \sum_{i}p^{k}_{i}v_{i}"><br/>
<img src="https://render.githubusercontent.com/render/math?math=u^{k} = \tilde{v}^{k}_{I} %2B u^{k-1}">
</p>
Cuối cùng, xác suất của đáp án sẽ được tính như sau: <br/>
<p align="center">
<img src="https://render.githubusercontent.com/render/math?math=p_{ans}=softmax(W_{u}u^{K} %2B b_{u})">
</p>

### 4. Điều kiện để là một local minima ?

### 5. Bàn luận.
Optimization là một phần không thể thiếu trong các mô hình Machine Learning, Deep Learning hiện nay. Việc hiểu rõ được các mô hình optmization sẽ giúp bạn có cái nhìn rõ hơn về việc tại sao model của bạn được tối ưu và bạn có thể đọc paper dễ hơn ví dụ như hiện nay có rất nhiều paper về các Optimization mới như Radam, Adam-Belief, KFAC, etc.

### 6. Tham khảo.
[Algorithms for Optimization, Mykel J.Kochenderfer, Tim A.Wheeler]()<br/>
[Code](https://github.com/ngthanhtin/optimization_algorithm): Repo này là mình tổng hợp lại các thuật toán Optimization trong sách, được viết bằng Julia và Python, nếu bạn có hứng thú về code có thể ghé qua nhé.<br/>

<div style="text-align: right"> (Tín Nguyễn) </div>
