---
layout: post
title: Multiple Object Tracking with Kalman Filter and Hungary Algorithm
subtitle: object-tracking-with-kalman-filter-hungary-algorithm
gh-repo: daattali/beautiful-jekyll
gh-badge: [settings]
tags: [multiple-object-tracking, kalman-filter, hungary-algorithm]
comments: true
cover: "/blog/img/mot/mot.png"
---
Hi, xin chào các bạn, hôm này mình sẽ giới thiệu cho các bạn bài toán <b>Multiple Object Tracking</b> sử dụng <b>Kalman Filter</b> để theo vết đối tượng
và <b>thuật toán Hungary</b> để giải bài toán credit assignment, nói nôm na là làm sao gán từng tracker vào mỗi object sao cho chi phí là ít nhất.<br/>
Ngoài ra, ở bài này, mình chỉ sử dụng phương pháp <b>trừ nền (Background Subtraction)</b> để detect đối tượng động. Các bạn có thể thay thế bằng các thuật toán object detection khác như YOLO, Faster RCNN, Centernet, etc.<br/>
Link code: [Code](https://github.com/ngthanhtin/Muiltiple-Object-Tracking)

Nội dung chính sẽ bao gồm các phần sau:<br/>
<a href="#1. Giới thiệu bài toán Multiple Object Tracking">1. Giới thiệu bài toàn Multiple Object Tracking</a> <br/>
<a href="#2. Nguyên lý">2. Nguyên lý</a> <br/>
<a href="#3. Phương pháp">3. Phương pháp</a> <br/>
<a href="#3.2 Object Detection">3.1 Object Detection</a> <br/>
<a href="#3.2 Kalman Filter">3.2 Kalman Filter</a> <br/>
<a href="#3.3 Hungary Algorithm">3.3 Hungary Algorithm</a> <br/>
<a href="#4. Ứng dụng">5. Ứng dụng</a> <br/>
<a href="#5. Tham khảo">6. Tham khảo</a> <br/>

<section id="1. Giới thiệu bài toán Multiple Object Tracking">
<b>1. Giới thiệu bài toán Multiple Object Tracking</b>
</section>


<section id="2. Nguyên lý">
<b>2. Nguyên lý</b>
</section>


<section id="3. Phương pháp">
<b>3. Phương pháp</b>
</section>

<b>3.1 Object Detection</b><br/>

<b>3.2 Kalman Filter</b><br/>

<b>3.3 Hungary Algorithm</b><br/>


<section id="4. Ứng dụng">
<b>5. Ứng dụng</b>
</section>


<section id="5. Tham khảo">
<b>6. Tham khảo</b>
</section>
[Code](https://github.com/ngthanhtin/Muiltiple-Object-Tracking)<br/>

<div style="text-align: right"> (Tín Nguyễn) </div>