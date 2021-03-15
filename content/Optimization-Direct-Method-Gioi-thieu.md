---
title : "Optmization - Direct Method Introduction"
cover : "/blog/img/math_optimization/direct/cyclic_coordinate_descent/cover.jpg"
date : "2021-03-15"
tags : 
  - "math"
  - "direct-method"
  - "optmization"

---

Hi, xin chào các bạn, hôm này mình sẽ giới thiệu cho các bạn về một phương pháp <b>Optmization</b> khác nữa đó là <b>Direct Method</b>. Phương pháp này thường được gọi là <i>zero-order, black box, pattern search, hoặc derivative-free</i> methods vì nó không sử dụng đạo hàm để tìm được local minimum, mà phương pháp này sẽ dùng những cách khác để chọn ra hướng tìm kiếm ở mỗi step để tìm ra local minimum và làm sao để biết khi nào là convergence.

Nội dung chính sẽ bao gồm các phần sau: <br/>

1. Giới thiệu phương pháp Direct Method đầu tiên - Cyclic Coordinate Search.
2. Thuật toán.
3. Code.
4. Bàn luận.
5. Tham khảo.



### 1. Giới thiệu phương pháp Direct Method đầu tiên - Cyclic Coordinate Search.
Cyclic Coordinate Search, hay còn được gọi là Coordinate Search, Taxicab Search, là thuật toán tối ưu tại mỗi thời điểm, chọn ra một hướng tọa độ và tối ưu hàm f(x) theo hướng đó.<br/>
Ta mô hình hóa như sau, ta có hàm f(x) như sau:
<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=f(x) = f(x_{1}, x_{2}, x_{3},..., x_{n})"><br/>
</p>
Ở step thứ nhất, ta có <img src="https://render.githubusercontent.com/render/math?math=x^{(0)} = (x_{1}^{0}, x_{2}^{0}, x_{3}^{0},..., x_{n}^{0})"> và ta chọn <img src="https://render.githubusercontent.com/render/math?math=x_{1}^{1}}"> làm hướng để tối ưu và thu được:<br/>
<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=x^{(1)} = \arg\min_{x_{1}^{0}} f(x_{1}^{0}, x_{2}^{0}, x_{3}^{0},..., x_{n}^{0})"><br/>
  <img src="https://render.githubusercontent.com/render/math?math=x^{(1)} = (x_{1}^{1}, x_{2}^{1}, x_{3}^{1},..., x_{n}^{1})"><br/>
</p>
Sau đó, tới step thứ 2, ta lấy <img src="https://render.githubusercontent.com/render/math?math=x_{2}^{1}}"> làm hướng để tối ưu và lại thu được:<br/>
<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=x^{(2)} = \arg\min_{x_{2}^{1}} f(x_{1}^{1}, x_{2}^{1}, x_{3}^{1},..., x_{n}^{1})"><br/>
  <img src="https://render.githubusercontent.com/render/math?math=x^{(2)} = (x_{1}^{2}, x_{2}^{2}, x_{3}^{2},..., x_{n}^{2})"><br/>
</p>


### 2. Thuật toán.


### 3. Code.


### 4. Bàn luận.

### 5. Tham khảo.


### 6. Tham khảo.
[Algorithms for Optimization, Mykel J.Kochenderfer, Tim A.Wheeler]()<br/>
[Code](https://github.com/ngthanhtin/optimization_algorithm): Repo này là mình tổng hợp lại các thuật toán Optimization trong sách, được viết bằng Julia và Python, nếu bạn có hứng thú về code có thể ghé qua nhé.<br/>

<div style="text-align: right"> (Tín Nguyễn) </div>
