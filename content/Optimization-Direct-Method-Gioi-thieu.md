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
Ở step thứ nhất, ta có <img src="https://render.githubusercontent.com/render/math?math=x^{(0)} = (x_{1}^{0}, x_{2}^{0}, x_{3}^{0},..., x_{n}^{0})"> và ta chọn <img src="https://render.githubusercontent.com/render/math?math=x_{1}^{0}"> làm hướng để tối ưu và thu được:<br/>
<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=x^{(1)} = \arg\min_{x_{1}^{0}} f(x_{1}^{0}, x_{2}^{0}, x_{3}^{0},..., x_{n}^{0})"><br/>
  <img src="https://render.githubusercontent.com/render/math?math=x^{(1)} = (x_{1}^{1}, x_{2}^{1}, x_{3}^{1},..., x_{n}^{1})"><br/>
</p>
Sau đó, tới step thứ 2, ta lấy <img src="https://render.githubusercontent.com/render/math?math=x_{2}^{1}"> làm hướng để tối ưu và lại thu được:<br/>
<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=x^{(2)} = \arg\min_{x_{2}^{1}} f(x_{1}^{1}, x_{2}^{1}, x_{3}^{1},..., x_{n}^{1})"><br/>
  <img src="https://render.githubusercontent.com/render/math?math=x^{(2)} = (x_{1}^{2}, x_{2}^{2}, x_{3}^{2},..., x_{n}^{2})"><br/>
</p>

Đối với thuật toán Cylic Coordinate Search, tại mỗi thời điểm, ta chỉ search trên một hướng, nghĩa là ở step 1 ở trên ta sẽ search theo một vector đơn vị là [1, 0, 0,..., 0], step 2 sẽ là [0, 1, 0,..., 0].<br/>
Và để ý rằng, các vector này là orthogonal với nhau, nghĩa là <b>aT.a = 0</b>.

<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/cyclic_coordinate_descent/coordinate_search.png?raw=true">
</p>
<div style="text-align: center">Mô tả quá trình optimize của thuật toán trên không gian 2 chiều.</div>

### 2. Thuật toán.
Ban đầu, ta sẽ khởi tạo một tập <img src="https://render.githubusercontent.com/render/math?math=x = (x_{1}, x_{2}, x_{3},..., x_{n})">, sau đó, với mỗi direction ta thực hiện line search optimization trên chiều đó và thu được một tập <img src="https://render.githubusercontent.com/render/math?math=x_{new} ">. <br/>
Bước thứ 2 là tính step size, nghĩa là lấy norm của tập x ban đầu và x new vừa tìm được.<br/>
Điều kiện dừng là khi step size nhỏ hơn một giá trị threshold nào đó.

Ngoài ra, ta còn có phiên bản acceleration của thuật toán này, đó là sau khi thực hiện line search trên tất cả các chiều để tìm ra tập <img src="https://render.githubusercontent.com/render/math?math=x_{new} "> thì thuật toán sẽ thực hiện thêm một lần line search nữa trước khi tính norm, nhưng lần này thực hiện trên một chiều đó là x-x_new. <br/>
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/cyclic_coordinate_descent/accelerated.png?raw=true">
</p>
<div style="text-align: center">Mô tả quá trình optimize của thuật toán Accelerated.</div>

### 3. Code.
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/cyclic_coordinate_descent/code1.png?raw=true">
</p>
<div style="text-align: center">Code của thuật toán Cyclic Coordinate Search.</div>

<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/cyclic_coordinate_descent/code2.png?raw=true">
</p>
<div style="text-align: center">Code của thuật toán Accelerated Cyclic Coordinate Search.</div>

### 4. Bàn luận.
Mặc dù ý tưởng của Coordinate rất hay, giúp tránh được việc tính toán gradient, nhưng cũng có trường hợp thuật toán ko thể optimize được ví dụ như hình sau:<br/>
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/cyclic_coordinate_descent/stuck.png?raw=true">
</p>
<div style="text-align: center">Trường hợp cyclic coordinate descent bị stuck.</div>
Ta có thể thấy, di chuyển theo hướng nào thì f(x) cũng sẽ tăng và không thể nào tiến vào local minimum được. Và cũng có thể thấy khi di chuyển diagonally (chéo) thì x có thể tiến vào local nhưng ở thuật toán này ko cho phép điều đó, ở các phần sau mình sẽ giới thiệu những phương pháp khác khắc phục vấn đề này nhé.<br/>

### 5. Tham khảo.
[Algorithms for Optimization, Mykel J.Kochenderfer, Tim A.Wheeler]()<br/>
[Code](https://github.com/ngthanhtin/optimization_algorithm): Repo này là mình tổng hợp lại các thuật toán Optimization trong sách, được viết bằng Julia và Python, nếu bạn có hứng thú về code có thể ghé qua nhé.<br/>

<div style="text-align: right"> (Tín Nguyễn) </div>
