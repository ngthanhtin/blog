---
title : "Optmization - Direct Method: Divided Rectangle (DIRECT)"
cover : "/blog/img/math_optimization/direct/direct/cover.png"
date : "2021-03-15"
tags : 
  - "math"
  - "direct-method"
  - "optmization"
  - "divided-rectangle"

---

Hi, xin chào các bạn, hôm này mình sẽ giới thiệu cho các bạn về một phương pháp <b>Optmization</b> bằng <b>Direct Method</b> đó là <b>DIvided RECTangle (DIRECT)</b>.

Nội dung chính sẽ bao gồm các phần sau: <br/>

1. Giới thiệu phương pháp Divided Rectangle (DIRECT).
2. Thuật toán.
3. Code.
4. Bàn luận.
5. Tham khảo.



### 1. Giới thiệu phương pháp Divided Rectangle (DIRECT).
Thuật toán DIRECT là một thuật toán sampling, nghĩa là nó ko dùng thông tin về gradient để tìm điểm design point tiếp theo. Thuật toán này được dùng khi tối ưu những bài toán có ràng buộc (bound constraints) và một real-value objective function.

#### 1.1 Lipschitz Optimization
Lipschitz Continuity được định nghĩa như sau:<br/>
Nếu f là Lipschitz continuous trong một không gian X với Lipschitz constant alpha > 0 thì: <br/>
<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=|f(x)-f(x^{'})| \leq \alpha|x-x^{'}| \forall x, x^{'} \in X">
</p>
Nếu hàm f có đặc tính này thì ta có thể tìm được minimum của f. Thuật toán Shubert-Piyavskii là một trong những thuật toán như vậy.<br/>
Ví dụ: <br/>
Giả sử ta có <img src="https://render.githubusercontent.com/render/math?math=M = [a,b] \subset R^{1}">, ta có thể xây dựng 2 bất đẳng thức dựa vào Lipschitz continuos:<br/>
 <p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=f(x) \geq f(a) - \alpha(x-a)"><br/>
  <img src="https://render.githubusercontent.com/render/math?math=f(x) \geq f(b) %2B \alpha(x-b)">
</p>
Dựa vào 2 bất đẳng thức trên ta có thể vẽ được 2 đường thẳng trên đồ thị như sau: <br/>
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/direct/shubert.png?raw=true">
</p>
Điểm giao nhau giữa 2 đường thẳng là điểm minimum được ước lượng ở step đầu tiên.<br/>
Sau đó, thuật toán Shubert tiếp tục ước lượng minimum trên đoạn [a, x1] và [x1, b].
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/direct/shubert_iteration.png?raw=true">
</p>

Có nhiều vấn đề đối với thuật toán Shubert này. Thứ nhất, thuật toán này ko tổng quát hóa lên khi số chiều lớn hơn 1. Thứ hai, Lipschitz constant thường ko được xác định hoặc ước lượng hợp lý. Nhiều ứng dụng công nghiệp hiện nay có thể ko có đặc tính của Lipschitz trong domain của nó. Cho dù có ước lượng được Lipschitz constant cũng sẽ dẫn đến kết quả xấu nếu việc ước lượng ko tốt.<br/>
Thuật toán DIRECT giải quyết được 2 vấn đề đó, thứ nhất nó có thể tổng quát hóa trên không gian nhiều chiều, thứ hai nó không cần phải ước lượng Lipschitz constant hoặc objective function ko cần phải có đặc tính Lipschitz continuality.

Ví dụ về việc thay đổi Lipschitz constant như hình sau:
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/direct/lipschitz_constant.png?raw=true">
</p>
<div style="text-align: center">Sự thay đổi của Lipschitz constant dẫn đến minimum cũng thay đổi, khi constant càng bé, design points càng gần f(x) và ngược lại.</div>

#### 1.2 Univariate DIRECT
Đối với không gian 1 chiều, DIRECT sẽ chia một đoạn (interval) làm 3, sau đó tính objective function ở điểm giữa của đoạn này. Chú ý rằng cách làm này khác với thuật toán Shubert đã nói ở trên, đối với thuật toán Shubert, điểm được sample là giao điểm của 2 đoạn thẳng được tạo thành từ 2 bất đẳng thức. <br/>
Cho một đoạn [a,b], điểm giữa là c = (a+b)/2, chặn dưới thấp nhất (lowest bound) sẽ là f(c):<br/>
<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=f(x) \geq f(c) - \alpha|x-c|">
</p>
Ở đây, chúng ta ko cần biết giá trị của alpha. Ta có thể thấy, nếu x = b thì ta sẽ tìm được lowest bound trên đoạn interval này, và giá trị đó sẽ là f(c) - alpha*(b-a)/2, và điểm đó nằm trên edges của interval đó. Hình minh họa như sau: <br/>
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/direct/univariate_lowest_bound.jpg?raw=true">
</p>
Mặc dù chúng ta ko biết giá trị alpha, nhưng chúng ta có thể suy luận ra rằng lower bound của interval này có thấp hơn lower bound của interval khác hay không. Ví dụ, chúng ta có 2 interval cùng chiều dài, giả sử interval thứ 1 có lower evaluation tại center (f(c)) thấp hơn của interval thứ 2, vì vậy, lower bound của interval thứ 1 sẽ thấp hơn lower bound của interval thứ 2. Mặc dù ko dám chắc rằng interval thứ 1 có chứa minimizer hay không, nhưng nó định hướng cho chúng ta cần phải tập trung tìm kiếm minimizer ở vùng nào.

Ví dụ, chúng ta có một hàm như sau <b>f(x) = sin(x) + sin(2x) + sin(4x) + sin(8x)</b>, xét trên interval [−2, 2] có một global minimizer gần với giá trị −0.272, ta minh họa quá trình thuật toán DIRECT tối ưu như sau:<br/>
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/direct/univariate_example.png?raw=true">
</p>
<div style="text-align: center">Những intervals được chọn để chia làm 3 là những đoạn màu xanh. Hình bên trái thể hiện cho ta thấy những interval và hàm objective. Hình bên phải thể hiện các intervals bằng các points, trong đó chiều ngang là interval hafl-width (b-a)/2, chiều dọc là interval evaluation f(c)</div>
<div style="text-align: center">Hàm số này có rất nhiều minimum như trên, vì vậy rất khó để optimize.</div>

#### 1.3 Multivariate DIRECT
Trong không gian nhiều chiều, cụ thể là 2 chiều, thuật toán chia các hình chữ nhật (rectangles) thay vì chia các intervals làm ba. Còn đối với số chiều > 2, ta chia các hyber-rectangles ra làm ba. Nhưng trước khi tiến hành chia cắt, DIRECT phải chuẩn hóa search space về  dạng unit hypercube.<br/>
Thứ tự chia cắt theo hướng nào rất quan trọng trong không gian nhiều chiều như trong mô tả sau:<br/>
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/direct/multivariate_order.png?raw=true">
</p>
Khi chia cắt một vùng ko có các chiều có chiều dài như nhau, chỉ có chiều dài nhất được chia cắt.<br/>
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/direct/multivariate_hyper_rectangle.png?raw=true">
</p>
<div style="text-align: center">DIRECT chỉ chia cắt theo chiều dài nhất của hyper rectangles.</div>
Tập các interval tối ưu cũng được tính như là ở trường hợp one dimension. Lower bound cho mỗi hyper-rectangle được tính dựa trên longest
side length và center value.

Ví dụ, DIRECT sau 16 vòng lặp trên hàm Branin:<br/>
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/direct/cover.png?raw=true">
</p>
với hàm Branin như sau:<br/>
<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=f(x) = \alpha(x_{2} - bx^{2} %2B cx_{1} - r)^{2} %2B s(1-t)cos(x_{1}) %2B + s">
</p>

### 2. Thuật toán.
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/direct/flow_code.png?raw=true">
</p>
<div style="text-align: center">Step của thuật toán DIRECT.</div>

### 3. Code.
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/direct/main_code.png?raw=true">
</p>
<div style="text-align: center">Thuật toán DIRECT.</div>

<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/direct/hybercube.png?raw=true">
</p>
<div style="text-align: center">Hybercube function.</div>

<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/direct/data_structure.png?raw=true">
</p>
<div style="text-align: center">Data Structure.</div>

<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/direct/opt_interval.png?raw=true">
</p>
<div style="text-align: center">Interval.</div>

<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/direct/divide.png?raw=true">
</p>
<div style="text-align: center">Divide interval.</div>


### 4. Bàn luận.
Thuật toán DIRECT giải quyết được yếu điểm của thuật toán Shubert đó là cần phải biết Lipschitz constant và tổng quát hóa hơn thuật toán Shubert khi có thể optimize cho không gian nhiều chiều.<br/>


### 5. Tham khảo.
[Algorithms for Optimization, Mykel J.Kochenderfer, Tim A.Wheeler]()<br/>
[Code](https://github.com/ngthanhtin/optimization_algorithm): Repo này là mình tổng hợp lại các thuật toán Optimization trong sách, được viết bằng Julia và Python, nếu bạn có hứng thú về code có thể ghé qua nhé.<br/>

<div style="text-align: right"> (Tín Nguyễn) </div>
