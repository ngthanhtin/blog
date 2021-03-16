---
title : "Optmization - Direct Method: Hooke Jeeves and Pattern Search"
cover : "/blog/img/math_optimization/direct/pattern_search/cover.png"
date : "2021-03-15"
tags : 
  - "math"
  - "direct-method"
  - "optmization"
  - "pattern-search"
  - "hooke-jeeves"

---

Hi, xin chào các bạn, hôm này mình sẽ giới thiệu cho các bạn thêm về một phương pháp <b>Optmization</b> bằng <b>Direct Method</b> đó là <b>Hooke-Jeeves method</b> và <b>Pattern Search</b>.<br/>
Trước khi bước vào nội dung, nếu bạn nào chưa biết Optimization là gì hoặc phương pháp Direct là gì có thể ghé qua 2 bài post sau:<br/>
Optimization: https://ngthanhtin.github.io/blog/optimization-gioi-thieu/</br>
Direct Method: https://ngthanhtin.github.io/blog/optimization-direct-method-gioi-thieu/</br>

Nội dung chính sẽ bao gồm các phần sau: <br/>

1. Thuật toán Hooke-Jeeves.
2. Thuật toán Generalized Pattern Search.
3. Code.
4. Bàn luận.
5. Tham khảo.



### 1. Thuật toán Hooke-Jeeves.
Ở thuật toán Hooke-Jeeves, tại mỗi step, thuật toán sẽ evaluate <img src="https://render.githubusercontent.com/render/math?math=f(x)"> và <img src="https://render.githubusercontent.com/render/math?math=f(x + \alpha e^{i})">.<br/>
Với <img src="https://render.githubusercontent.com/render/math?math=\alpha"> là step size, <img src="https://render.githubusercontent.com/render/math?math=e^{i}"> là coordinate direction giống như [Cyclic Coordinate Search](https://ngthanhtin.github.io/blog/algorithms_for_optimization/direct_method/optimization-direct-method-gioi-thieu/).<br/>
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/pattern_search/hooke_jeeves.png?raw=true">
</p>
<div style="text-align: center">Ta có thể thấy, x tại mỗi thời điểm sẽ tìm kiếm trên các hướng cho đến khi không còn một sự improve nào nữa (từ trái qua phải).</div>


### 2. Thuật toán Generalized Pattern Search.

### 3. Code.
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/pattern_search/code_hooke_jeeves.png?raw=true">
</p>
<div style="text-align: center">Ta có thể thấy, direction ở đây là (-1, 1) và mỗi step thuật toán phải evaluate (so sánh y' với y_best) 2 lần.</div>

<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/pattern_search/code_pattern_search.png?raw=true">
</p>
<div style="text-align: center">Code của pattern search.</div>

### 4. Bàn luận.
Ở thuật toán Hooke-Jevees, ở mỗi step nó phải evaluate 2*n lần (n là số direction) dẫn đến chậm hơn nếu n lớn.<br/>

### 5. Tham khảo.
[Algorithms for Optimization, Mykel J.Kochenderfer, Tim A.Wheeler]()<br/>
[Code](https://github.com/ngthanhtin/optimization_algorithm): Repo này là mình tổng hợp lại các thuật toán Optimization trong sách, được viết bằng Julia và Python, nếu bạn có hứng thú về code có thể ghé qua nhé.<br/>

<div style="text-align: right"> (Tín Nguyễn) </div>
