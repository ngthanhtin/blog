---
title : "Optmization - Direct Method: Nelder Mead Simplex"
cover : "/blog/img/math_optimization/direct/nelder_mead/cover.jpg"
date : "2021-03-18"
tags : 
  - "math"
  - "direct-method"
  - "optmization"
  - "simplex"
  - "nelder-mead"

---

Hi, xin chào các bạn, hôm này mình sẽ giới thiệu cho các bạn thêm về một phương pháp <b>Optmization</b> bằng <b>Direct Method</b> đó là <b>Nelder Mead Simplex</b>. Phương pháp này được 2 nhà khoa học John Nelder và Roger Mead tạo ra trong một paper mang tên A Simplex Method for Function Minimization năm 1965.

Nội dung chính sẽ bao gồm các phần sau: <br/>

1. Giới thiệu phương pháp Nelder-Mead-Simplex.
2. Thuật toán.
3. Code.
4. Bàn luận.
5. Tham khảo.



### 1. Giới thiệu phương pháp Nelder-Mead-Simplex.
#### 1.1 Simplex là gì ?
Simplex đơn giản là <b>phiên bản n-chiều của một tam giác</br>.
Ví dụ:<br/>
1-dimension: là một điểm.<br/>
2-dimension: là một đường thẳng.<br/>
3-dimension: là một tam giác.<br/>
4-dimension: là một tứ diện.<br/>
<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/nelder_mead/simplexes.jpg?raw=true">
</p>
<div style="text-align: center">Simplexes.</div>

#### 1.2 Thuật toán Nelder-Mead
Nelder-Mead bắt đầu với một simplex ngẫu nhiên. Tại mỗi step, nó sẽ mở rộng (expand) hoặc thu hẹp (contract) simplex đó cho tới khi nó tới vùng optimal.<br/>
Dưới đây là một mô tả của thuật toán Nelder-Mead.

<video width="320" height="240" autoplay>
  <source src="https://www.youtube.com/watch?v=HUqLxHfxWqU&ab_channel=brainbrian123" type="video/mp4">
</video>
<div style="text-align: center">Mô tả quá trình Nelder Mead Algorithm 2D.</div>

### 2. Thuật toán.
##### 2.1 Reflection

##### 2.2 Expansion

##### 2.3 Contraction

##### 2.4 Shrinkage

### 3. Code.

### 4. Bàn luận.
Nelder-Mead thường được dùng cho parameter selection trong Machine Learning, Data Science vì thuật toán optimize khá nhanh và hiệu quả.<br/>
Mặc dù Nelder-Mead có thể optimize khá nhanh, nhưng khi simplex tiến vào một vùng của local optimum, nó sẽ liên tục contracting/shrinking, thay vì đi tìm kiếm thêm ở những space khác, vì vậy, nó trở nên khó có thể cải thiện nhiều hơn trong trường hợp này và đòi hỏi phải restart lại thuật toán.

### 5. Tham khảo.
[Algorithms for Optimization, Mykel J.Kochenderfer, Tim A.Wheeler]()<br/>
[Code](https://github.com/ngthanhtin/optimization_algorithm): Repo này là mình tổng hợp lại các thuật toán Optimization trong sách, được viết bằng Julia và Python, nếu bạn có hứng thú về code có thể ghé qua nhé.<br/>

<div style="text-align: right"> (Tín Nguyễn) </div>
