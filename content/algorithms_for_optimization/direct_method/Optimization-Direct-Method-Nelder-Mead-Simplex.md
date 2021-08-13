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
categories : "Optimization Math"
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
Simplex đơn giản là <b>phiên bản n-chiều của một tam giác.</b></br>
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

<iframe width="795" height="672" src="https://www.youtube.com/embed/HUqLxHfxWqU" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen>
</iframe>
<div style="text-align: center">Mô tả quá trình Nelder Mead Algorithm 2D.</div>

### 2. Thuật toán.
Giả sử chúng ta đang optimize trong không gian n-chiều. Một Simplex bao gồm n+1 vertex: <img src="https://render.githubusercontent.com/render/math?math=[x_{1}, x_{2}, x_{3},..., x_{n %2B 1}] ">. Hàm mà chúng ta đang optimize là f(x). Thuật toán Nelder-Mead (NMM) cũng được gọi là thuật toán Downhill Simplex và bao gồm các bước sau:

#### 2.0 Preprocess
Ta sẽ sắp xếp các đỉnh từ nhỏ đến tới lớn dựa trên function value (tức là f(x_i)), và gọi đỉnh có giá trị nhỏ nhất, nhỏ nhất thứ hai và cao nhất lần lượt là xh, xs, xl. Tính centroid của các đỉnh là <img src="https://render.githubusercontent.com/render/math?math=\bar{x}">.
<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=\bar{x} = \frac{1}{n %2B 1}\sum_{i \neq h} x_{i}">
</p>

#### 2.1 Reflection
<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=xr = \bar{x} %2B \alpha * (\bar{x} - xh)">
</p>
xr là một điểm trên đường thẳng nối <img src="https://render.githubusercontent.com/render/math?math=\bar{x} và xh">, nó có mục đích là dịch chuyển simplex từ vùng sub-optimal của xh sang một vùng tốt hơn.<br/>
<img src="https://render.githubusercontent.com/render/math?math=\alpha > 0"> là hệ số reflection và thuờng được đặt bằng 1.

<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/nelder_mead/reflection.png?raw=true">
</p>
<div style="text-align: center">Quá trình Reflection.</div>

Ta có thể thấy, nếu <img src="https://render.githubusercontent.com/render/math?math=f(xs) < f(xr) \leq f(xl)"> : nghĩa là xr tốt hơn xh, chúng ta sẽ thay xh bằng xr trong simplex.

#### 2.2 Expansion
<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=xe = \bar{x} %2B \beta * (xr - \bar{x})">
</p>
<img src="https://render.githubusercontent.com/render/math?math=\beta > 0"> là hệ số expansion, <img src="https://render.githubusercontent.com/render/math?math=\beta > max(1, alpha)"> và thường được đặt bằng 2.

Nếu xr tốt hơn xl (f(xr) > f(xl)),thuật toán sẽ cố gắng đưa xr đi xa hơn. Nghĩa là, đưa xr dịch chuyển theo hướng của xr từ <img src="https://render.githubusercontent.com/render/math?math=\bar{x}"> để tìm kiếm một điểm tốt hơn.

<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/nelder_mead/expansion.png?raw=true">
</p>
<div style="text-align: center">Quá trình Expansion.</div>

Sau đó, chúng ta sẽ thay thế xh bằng 1 trong 2 điểm xe và xr trong simplex. Cách thứ nhất, chỉ cần thay điểm xh bằng điểm tốt hơn trong 2 điểm xe và xr. Cách thứ 2, là thay thế xe với xl nếu (f(xe) > f(xl)) cho dù xe có tốt hơn xr hay không, ý tưởng của cách thứ 2 đến từ việc Expansion luôn làm cho simplex rộng ra, nghĩa là không gian tìm kiếm cũng nhiều hơn.

#### 2.3 Contraction
<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=xc = \bar{x} %2B \gamma * (xr - \bar{x})">
</p>
<img src="https://render.githubusercontent.com/render/math?math=\gamma"> được gọi là hệ số contraction và thường được đặt bằng 0.5, <img src="https://render.githubusercontent.com/render/math?math=\gamma"> thuộc (0, 1).

Giả sử xr tệ hơn xs. Nghĩa là khi mở rộng theo hướng xr sẽ làm tệ đi, vì vậy simplex sẽ co lại, centroid sẽ thu lại thành <img src="https://render.githubusercontent.com/render/math?math=\bar{x}">.

<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/nelder_mead/contraction.png?raw=true">
</p>
<div style="text-align: center">Quá trình Contraction.</div>

Nếu f(<img src="https://render.githubusercontent.com/render/math?math=\bar{x}">) > f(xh), nghĩa là điểm contracted point tốt hơn điểm tệ nhất, chúng ta sẽ thay xh bằng <img src="https://render.githubusercontent.com/render/math?math=\bar{x}"> trong simplex. Tuy nhiên, nếu điều này không xảy ra, ta sẽ dùng phương án cuối cùng là Shrinkage.

#### 2.4 Shrinkage
Đây là quá trình tất cả các điểm <img src="https://render.githubusercontent.com/render/math?math=xj"> được dịch chuyển tới gần điểm xl (best point), thường khoảng cách giữa các điểm tới xl sẽ bị giảm một nửa.

<p align="center">
  <img src="https://render.githubusercontent.com/render/math?math=xj = xl %2B \delta * (xj - xl)">
</p>

<img src="https://render.githubusercontent.com/render/math?math=\delta"> được gọi là shrinkage parameter, thường được đặt bằng 0.5.<br/>


<p align="center">
  <img src="https://github.com/ngthanhtin/blog/blob/master/static/img/math_optimization/direct/nelder_mead/shrinkage.png?raw=true">
</p>
<div style="text-align: center">Quá trình Shrinkage.</div>

Đây là bước tốn kém nhất vì ta phải thay thế nhiều điểm trong simplex. Tuy nhiên, theo nhiều kinh nghiệm cho thấy, điều này hiếm khi xảy ra cho nên nhiều implementation thường bỏ qua bước này.

#### 2.5 Điều kiện dừng
1. Vượt qua số iteration định trước.

2. Simplex đạt tới maximum size định trước.

3. Best solution tới giới hạn, nghĩa là không thể improve được nữa.

* Vậy làm sao để khởi tạo Simplex đầu tiên?<br/>
Ta muốn khởi tạo một Simplex với n+1 điểm. Và điểm đầu tiên x1 sẽ được khởi tạo bằng 2 cách:<br/>
a. Người dùng tự nhập.
b. Chọn một điểm ngẫu nhiên.
Sau đó, chúng ta phải khởi tạo thêm n điểm còn lại, các điểm này sẽ được tạo dựa trên x1, với một khoảng cách nhỏ tho hướng của mỗi chiều. Ví dụ:
x(i+1) = x1 + h(x1, i) * ui
ui là unit vector theo chiều i.<br/>
(x1, i) được định nghĩa như sau:<br/>
h(x1, i) = 0.05 nếu coefficient của ui trong x_1 là non-zero.<br/>
h(x1, i) = 0.00025 nếu coefficient của ui trong x1 là zero.<br/>

### 3. Code.
Update...

### 4. Bàn luận.
Nelder-Mead thường được dùng cho parameter selection trong Machine Learning, Data Science vì thuật toán optimize khá nhanh và hiệu quả.<br/>
Mặc dù Nelder-Mead có thể optimize khá nhanh, nhưng khi simplex tiến vào một vùng của local optimum, nó sẽ liên tục contracting/shrinking, thay vì đi tìm kiếm thêm ở những space khác, vì vậy, nó trở nên khó có thể cải thiện nhiều hơn trong trường hợp này và đòi hỏi phải restart lại thuật toán.

### 5. Tham khảo.
[Algorithms for Optimization, Mykel J.Kochenderfer, Tim A.Wheeler]()<br/>
[Code](https://github.com/ngthanhtin/optimization_algorithm): Repo này là mình tổng hợp lại các thuật toán Optimization trong sách, được viết bằng Julia và Python, nếu bạn có hứng thú về code có thể ghé qua nhé.<br/>

<div style="text-align: right"> (Tín Nguyễn) </div>
