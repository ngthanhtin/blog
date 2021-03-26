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
  <img src="https://render.githubusercontent.com/render/math?math=|f(x)-f(x')| \leq \alpha|x-x'|">
</p>

There are several problems with this type of algorithm. Since the idea of endpoints
does not translate well into higher dimensions, the algorithm does not have an intuitive
generalization for N > 1. Second, the Lipschitz constant frequently can not be determined
or reasonable estimated. Many simulations with industrial applications may not even be
Lipschitz continous throughout their domains. Even if the Lipschitz constant can be estimated, a poor choice can lead to poor results. If the estimate is too low, the result may not
be a minimum of f, and if the choice is too large, the convergence of the Shubert algorithm
will be slow.

Thuật toán DIRECT ko yêu cầu phải biết trước Lipschitz constant, vì vậy ta có thể lựa chọn constant để test như hình sau:
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/direct/lipschitz_constant.png?raw=true">
</p>
<div style="text-align: center">Sự thay đổi của Lipschitz constant dẫn đến minimum cũng thay đổi, khi constant càng bé, design points càng gần f(x) và ngược lại.</div>


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



### 5. Tham khảo.
[Algorithms for Optimization, Mykel J.Kochenderfer, Tim A.Wheeler]()<br/>
[Code](https://github.com/ngthanhtin/optimization_algorithm): Repo này là mình tổng hợp lại các thuật toán Optimization trong sách, được viết bằng Julia và Python, nếu bạn có hứng thú về code có thể ghé qua nhé.<br/>

<div style="text-align: right"> (Tín Nguyễn) </div>
