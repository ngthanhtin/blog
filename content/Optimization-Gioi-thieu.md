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
Optmization được hiểu là quá trình bao gồm việc đánh giá thiết kế tốt hay xấu, và điều chỉnh lại thiết kế nếu như chúng chưa đạt yêu cầu.
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/introduction.png?raw=true">
</p>
Còn khi mô hình hóa thành toán học, nó sẽ được viết như sau:
<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=\minimize_{x} f(x)"><br/>
  <img src="https://render.githubusercontent.com/render/math?math=\subject to x\in X"><br/>
</p>
Công thức này có nghĩa là tìm x sao cho hàm f(x) là nhỏ nhất với x nằm trong tập X.<br/>
f(x) được gọi là <b>objective function</b>, X được gọi là <b>feasible set</b>, và x được gọi là <b>design point</b>.<br/>
Nếu trong không gian n-chiều, x sẽ được biểu diễn bằng một vector cột [x1, x2, x3,.., xn]. Nếu một tập các điểm x làm cho f(x) nhỏ nhất, ta gọi đó là <b>minimizer</b> hoặc <b>solution</b>, và được kí hiệu là x*.

Minimizer hay solution chính là local minimum mà ta cần tìm, nhưng tại sao không phải là global minimum?. Câu trả lời đơn giản là có rất nhiều hàm phức tạp, vì thế việc tìm global minimum là rất khó, vì vậy ta chỉ cần tìm được local minimum. Ở các phần sau mình sẽ giới thiệu kĩ hơn về local minimum và điều kiện để đạt được nó.

### 2. Constraints ?
Constraint là tập feasible set đã nói ở trên. Nó có nghĩa là một tập X chứa các giá trị có thể có của x. Lấy ví dụ bài toán sau:
<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=\minimize_{x_{1}, x_{2}} f(x_{1}, x_{2})"><br/>
  <img src="https://render.githubusercontent.com/render/math?math=\subject to x_{1} \ge 0"><br/>
  <img src="https://render.githubusercontent.com/render/math?math=\           x_{2} \ge 0"><br/>
  <img src="https://render.githubusercontent.com/render/math?math=\           x_{1} %2B x_{2} \le 1"><br/>
</p>
Ta có thể thấy, phần màu xanh ở hình dưới này chính là feasible set của bài toán trên.
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/constraints.png?raw=true">
</p>

### 3. Critical Points ?
Ở phần này, ta sẽ ôn lại một số loại điểm đặc biệt của một hàm.
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/critical_points.png?raw=true">
</p>
<b>Global minimum: </b>Điểm này là điểm tối ưu toàn cục. <br/>
<b>Strong local minimum: </b>Còn được gọi là <b>strict local minimizer</b>, nếu tồn tại <img src="https://render.githubusercontent.com/render/math?math=\delta"> > 0, f(x*) < f(x), khi và chỉ khi x* != x và ||x* - x|| < <img src="https://render.githubusercontent.com/render/math?math=\delta">.<br/>
<b>Weak local minimum: </b>Là local minimum không phải là strong local minimum.<br/>
Tại những điểm global, local minimum, f(x) sẽ có đạo hàm bậc nhất bằng 0. Tuy nhiên, điểm có đạo hàm bằng 0 chưa chắc là local minima, những điểm này còn được gọi là <b>stationary points</b>.<br/>
<b>Inflection points: </b>Là điểm (1) nếu có đạo hàm bằng 0 thì cũng không phải local minima, (2) là điểm mà đạo hàm bậc 2 f'' đổi dấu, (3) không nhất thiết phải có đạo hàm bằng 0.<br/>
Về inflection points, ta có những ví dụ sau:
Cho đồ thị của hàm f'(x) như sau, đâu là inflection points?
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/vd1.png?raw=true">
</p>
Ta có thể thấy những điểm mà f'' đổi dấu là (2,2), (4,0) và (6,2) (thỏa điều kiện 2). <br/>
Cũng có thể thấy điểm (4,0) có f' bằng 0, tuy nhiên ko phải là local minima (thỏa điều kiện 1). <br/>
Điểm (2,2) và (6,2) là inflection point vì làm f'' đổi dấu tuy nhiên f' khác 0 (thỏa điều kiện 3). <br/>

### 4. Điều kiện để là một local minima ?
#### 4.1 Hàm một biến Univariate.
Đối với hàm một biến, một điểm design point x được xem là local minima khi nó thỏa 2 điều kiện : <br/>
1. f'(x) = 0 <br/>
2. f''(x) > 0 (nếu ngược lại sẽ là local maximum) <br/>
Một điểm mà chỉ thỏa điều kiện 1 thì sẽ được gọi là stationary point (có nhắc ở trên).

#### 4.2 Hàm nhiều biến Multivariate.
Đối với hàm nhiều biến, một tập các design point x được xem là local minima khi nó thỏa 2 điều kiện : <br/>
1. <img src="https://render.githubusercontent.com/render/math?math=\Delta f(x) = 0">, hay còn gọi là ma trận Jaccobian.
2. <img src="https://render.githubusercontent.com/render/math?math=\Delta^{2} f(x) = 0"> f(x) là ma trận positive definite, hay còn gọi là ma trận Hessain.

Dưới đây là hình mô tả các loại điểm: 
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/local_maximum.png?raw=true">
</p>
Đây là local maxima, có gradient ở giữa bằng 0 và ma trận Hessain là ma trận negative definite.

<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/saddle.png?raw=true">
</p>
Đây là điểm <b>saddle (yên ngựa)</b>, điểm này có gradient ở giữa bằng 0 nhưng ko phải là local minima.

<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/bowl.png?raw=true">
</p>
Đây là điểm <b>bowl</b>, điểm này là local minima, có gradient ở giữa bằng 0 và ma trận Hessain là possitive definite.

### 5. Bàn luận.
Optimization là một phần không thể thiếu trong các mô hình Machine Learning, Deep Learning hiện nay. Việc hiểu rõ được các mô hình optmization sẽ giúp bạn có cái nhìn rõ hơn về việc tại sao model của bạn được tối ưu và bạn có thể đọc paper dễ hơn ví dụ như hiện nay có rất nhiều paper về các Optimization mới như Radam, Adam-Belief, KFAC, etc.

### 6. Tham khảo.
[Algorithms for Optimization, Mykel J.Kochenderfer, Tim A.Wheeler]()<br/>
[Code](https://github.com/ngthanhtin/optimization_algorithm): Repo này là mình tổng hợp lại các thuật toán Optimization trong sách, được viết bằng Julia và Python, nếu bạn có hứng thú về code có thể ghé qua nhé.<br/>

<div style="text-align: right"> (Tín Nguyễn) </div>