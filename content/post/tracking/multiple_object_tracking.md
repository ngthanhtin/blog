---
layout: post
title: Multiple Object Tracking with Kalman Filter and Hungary Algorithm
subtitle: object-tracking-with-kalman-filter-hungary-algorithm
gh-repo: daattali/beautiful-jekyll
gh-badge: [settings]
tags: [multiple-object-tracking]
categories: "Object Tracking"
comments: true
thumbnail: "/img/mot/mot.png"
date: "2021-02-04"
---
Hi, xin chào các bạn, hôm này mình sẽ giới thiệu cho các bạn bài toán <b>Multiple Object Tracking</b> sử dụng <b>Kalman Filter</b> để theo vết đối tượng
và <b>thuật toán Hungary</b> để giải bài toán credit assignment, nói nôm na là làm sao gán từng tracker vào mỗi object sao cho chi phí là ít nhất.<br/>
Ngoài ra, ở bài này, mình chỉ sử dụng phương pháp <b>trừ nền (Background Subtraction)</b> để detect đối tượng động. Các bạn có thể thay thế bằng các thuật toán object detection khác như YOLO, Faster RCNN, Centernet, etc.<br/>
Link code: Đây là code của thuật toán, đã được mình implement bằng C++, đây cũng là đồ án môn Computer Vision của mình hồi năm 3 tại trường đại học khoa học tự nhiên, Hồ Chí Minh. [Code](https://github.com/ngthanhtin/Muiltiple-Object-Tracking)

Nội dung chính sẽ bao gồm các phần sau:<br/>
<a href="#1. Giới thiệu bài toán Multiple Object Tracking">1. Giới thiệu bài toàn Multiple Object Tracking</a> <br/>
<a href="#2. Nguyên lý">2. Nguyên lý</a> <br/>
<a href="#3. Phương pháp">3. Phương pháp</a> <br/>
* <a href="#3.1 Background Generation">3.1 Background Generation</a><br/>
* <a href="#3.2 Object Detection">3.2 Object Detection</a> <br/>
* <a href="#3.3 Kalman Filter">3.3 Kalman Filter</a> <br/>
* <a href="#3.4 Hungary Algorithm">3.4 Hungary Algorithm</a> <br/>

<a href="#4. Ứng dụng">4. Ứng dụng</a> <br/>
<a href="#5. Tham khảo">5. Tham khảo</a> <br/>

<section id="1. Giới thiệu bài toán Multiple Object Tracking">
<b>1. Giới thiệu bài toán Multiple Object Tracking</b>
</section>
<p align="center">
  <img src="/img/mot/mot.png">
</p>
Đây đơn giản là bài toán theo vết nhiều đối tượng cùng một lúc.

<section id="2. Nguyên lý">
<b>2. Nguyên lý</b>
</section>
Nguyên lí của bài toán này vẫn là tìm kiếm nhiều đối tượng (object detection) và so khớp (matching) đối tượng với những đối tượng ở frame trước để xác định đâu là những đối tượng cần theo vết.

<section id="3. Phương pháp">
<b>3. Phương pháp</b>
</section>
Ở frame đầu tiên, ta sẽ dùng phương pháp trừ nền để phát hiện đối tượng và cũng sẽ được dùng ở những frame sau với mục đích detect đối tượng động.

Sau đó, ta sẽ dùng Kalman Filter (KF) để  dự đoán vị trí tiếp theo của từng đối tượng.

Để có thể dự đoán được vị trí bằng KF chính xác, ta phải thực hiện so khớp (matching). Ở bước này, ta phải xác định được đối tượng nào là đối tượng nào ở frame trước đó, và ta sẽ giải quyết bằng thuật toán Hungary Algorithm.

Cuối cùng là lặp lại quá trình trên đến khi video kết thúc.

* <b>3.1 Background Generation</b><br/>
Để  sử dụng phương pháp trừ nền, yêu cầu là background của chúng ta phải khá tĩnh, nếu background động thì phương pháp không còn tác dụng vì phương pháp này mục đích là để trích xuất được những đối tượng động. <br/>
Để trừ nền, ta đơn giản chỉ cần dùng background ở frame (t) - background ở frame (t-1). Tuy nhiên, mình đã thử và thấy có một phương pháp khác hay hơn, đó là Pixel Histrogram.<br/>
Pixel Histogram là phương pháp tái tạo nền, input sẽ là một histogram, output là nền được tái tạo dưới dạng grayscale.<br/>
Giả sử mỗi kích thước của một frame là w (width) và h (height). Ứng với mỗi pixel trên frame, ta sẽ có một histogram 256 chiều. Sau khi có được tất cả là (w x h) histogram, ta sẽ lấy ra giá trị màu lớn nhất ở mỗi histogram và đặt làm giá trị màu tại pixel tương ứng. Cuối cùng sẽ tái tạo được background.<br/>
Để tạo ra được từng histogram đó, ta cần phải lướt qua K frame của video, ứng với mỗi frame ta sẽ lướt từng pixel, lấy ra giá trị màu ở pixel đó và cuối cùng là tăng thêm 1 cho bin màu tương ứng của histogram.
Sau khi lướt hết K frame, ta sẽ có được (w x h) histogram.

* <b>3.2 Object Detection</b><br/>
Sau khi có được nền, ta sẽ bước đến giai đoạn detection & tracking.<br/>
Để detect những đối tượng động, mỗi lần process frame, ta sẽ lấy frame đó trừ cho nền đã được tái tạo bằng phương pháp ở trên. Phương pháp này rất đơn giản, nếu 2 đối tượng đi gần nhau quá sẽ bị dính và cho là 1 đối tượng nhưng được cái là nó cực kì nhanh =))<br/>
Đây là phương pháp đơn giản, vì hồi đó mới sinh viên, có biết deep learning đâu nên làm cách này haha. Bây giờ các bạn có thể  dùng YOLO hoặc Faster RCNN, CenterNet để detect cho chính xác hơn.
<p align="center">
  <img src="/img/mot/detect.png">
  <img src="/img/mot/detect_sub.png">
</p>

* <b>3.3 Kalman Filter</b><br/>
Kalman Filter là một mô hình lọc nhiễu, nó bao gồm 2 phần đó là dự đoán (predict) và cập nhật (update).
Ở bước predict, nó sẽ dự đoán vị trí của từng đối tượng, sau đó ở bước Update nó sẽ cập nhật, sửa lại cho đúng hơn.

* <b>3.4 Hungary Algorithm</b><br/>
Đây là thuật toán dùng để giải quyết <b>assignment problem</b>.
Assignment Problem là bài toán có N đối tượng, và M công việc, mỗi đối tượng khi làm một công việc nào đó sẽ có một chi phí nhất định. Và nhiệm vụ đó là làm sao gán mỗi đối tượng cho một công việc nào đó sao cho tổng chi phí là nhỏ nhất. <br/>
<p align="center">
  <img src="/img/mot/hungary.png">
</p>
Đối với bài toán tracking, ta có thể áp dụng như sau:<br/>
Ta có N tracker và M đối tượng detect được, và ta phải gán từng tracker với từng đối tượng sao cho chi phí là nhỏ nhất. Vậy chi phí ở đây là gì ?<br/>
Mỗi tracker đó chính là một Kalman Filter (KF), ở mỗi frame, KF sẽ dự đoán vị trí của đối tượng trước, gọi là p_predict, và ở mỗi frame ta lại detect đối tượng và ra được vị trí của chúng gọi là p_object. Vậy, khỏang cách giữa p_predict và p_object chính là chi phí. Ta sẽ đưa thông tin này qua thuật toán Hungary để giải quyết bài toán assignment, và KF sẽ cập nhật lại vị trí tracking của mình.

<section id="4. Ứng dụng">
<b>5. Ứng dụng và Bàn luận</b>
</section>
Bài toán này được ứng dụng trong theo vết đối tượng giao thông, đối tượng trong các siêu thị, cửa hàng, ...

Ở bài viết này, mình chỉ viết trình bày đơn giản các bước, chưa trình bày kĩ thuật toán cũng như có code minh họa vì mình không có thời gian nhiều để làm kĩ, mong các bạn thông cảm. Để hiểu hơn về bài viết, các bạn có thể tham khảo [link repo](https://github.com/ngthanhtin/Muiltiple-Object-Tracking) của mình nhé.<br/>
Nếu thấy hay, cho mình một star hoặc fork trên repo nhé. Thank you for reading.

<section id="5. Tham khảo">
<b>6. Tham khảo</b>
</section>

[Code](https://github.com/ngthanhtin/Muiltiple-Object-Tracking)<br/>

<div style="text-align: right"> (Tín Nguyễn) </div>