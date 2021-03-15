---
title : "Optmization - Direct Method - Powell Method"
cover : "/blog/img/math_optimization/direct/powell/cover.png"
date : "2021-03-15"
tags : 
  - "math"
  - "direct-method"
  - "optmization"
  - "powell"

---

Hi, xin chào các bạn, hôm này mình sẽ giới thiệu cho các bạn thêm về một phương pháp <b>Optmization</b> bằng <b>Direct Method</b> đó là <b>Powell Method</b>.
Trước khi bước vào nội dung, nếu bạn nào chưa biết Optimization là gì hoặc phương pháp Direct là gì có thể ghé qua 2 bài post sau:<br/>
Optimization: https://ngthanhtin.github.io/blog/optimization-gioi-thieu/</br>
Direct Method: https://ngthanhtin.github.io/blog/optimization-direct-method-gioi-thieu/</br>
Nội dung chính sẽ bao gồm các phần sau: <br/>

1. Giới thiệu phương pháp Powell.
2. Thuật toán.
3. Code.
4. Bàn luận.
5. Tham khảo.



### 1. Giới thiệu phương pháp Powell.
Powell method cho phép dùng line search trên những direction mà ko orthogonal với nhau như phương pháp Cyclic Coordinate Method.<br/>
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/powell/cover.png?raw=true">
</p>
Ta có thể thấy, hướng di chuyển của x không nhất thiết phải orthogonal với hướng cũ.

### 2. Thuật toán.
Đối với thuật toán này, ta vẫn sẽ thực hiện line search trên tất cả các direction như thuật toán Cyclic Coordinate và sẽ có được một tập <img src="https://render.githubusercontent.com/render/math?math=x_{new} ">. <br/>
Sau đó sẽ thực hiện line search trên (x_old - x_new)
Bước thứ 2 là tính step size, nghĩa là lấy norm của tập x ban đầu và x new vừa tìm được.<br/>
Điều kiện dừng là khi step size nhỏ hơn một giá trị threshold nào đó.


### 3. Code.
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/powell/code.png?raw=true">
</p>
<div style="text-align: center">Code của thuật toán Powell.</div>

### 4. Bàn luận.
Mặc dù thuật toán này giải quyết được vấn đề của Cyclic Coordinate Search nhưng nó đòi hỏi nhiều vòng lặp hơn thuật toán ban đầu.

### 5. Tham khảo.
[Algorithms for Optimization, Mykel J.Kochenderfer, Tim A.Wheeler]()<br/>
[Code](https://github.com/ngthanhtin/optimization_algorithm): Repo này là mình tổng hợp lại các thuật toán Optimization trong sách, được viết bằng Julia và Python, nếu bạn có hứng thú về code có thể ghé qua nhé.<br/>

<div style="text-align: right"> (Tín Nguyễn) </div>
