---
layout: post
title: Single Object Tracking with Particle Filter
subtitle: object-tracking-with-particle-filter
gh-repo: daattali/beautiful-jekyll
gh-badge: [settings]
tags: [single-object-tracking, particle-filter]
categories: "Object Tracking"
comments: true
cover: "/blog/img/sot/sot.png"
---
Hi, xin chào các bạn, hôm này mình sẽ giới thiệu cho các bạn bài toán <b>Single Object Tracking</b> sử dụng <b>Particle Filter</b><br/>
Link paper: [A New Framework of Moving Object Tracking based on Object Detection-Tracking with Removal of Moving Features using Stereo Camera and IMU](https://thesai.org/Downloads/Volume11No4/Paper_6-A_New_Framework_of_Moving_Object_Tracking.pdf)<br/>
Link code: [Code](https://github.com/ngthanhtin/Face_Tracking_ParticleFilter). Đây chỉ là code implementation của module face tracking bằng c++, tuy nhiên nó sẽ giúp bạn biết được khái quát particle filter được implement như thế nào.

Nội dung chính sẽ bao gồm các phần sau:<br/>
<a href="#1. Giới thiệu bài toán Single Object Tracking">1. Giới thiệu bài toàn Single Object Tracking</a> <br/>
<a href="#2. Nguyên lý">2. Nguyên lý</a> <br/>
<a href="#3. Phương pháp">3. Phương pháp</a> <br/>
<a href="#4. Giải thuật">4. Giải thuật</a> <br/>
<a href="#5. Ứng dụng">5. Ứng dụng</a> <br/>
<a href="#6. Tham khảo">6. Tham khảo</a> <br/>

<section id="1. Giới thiệu bài toán Single Object Tracking">
<b>1. Giới thiệu bài toán Single Object Tracking</b>
</section>
<p align="center">
  <img src="/blog/img/sot/sot.png">
</p>
Đối với bài toán Visual Object Tracking, nó được chia thành 2 dạng chính là Multiple Object Tracking, nghĩa là theo vết nhiều đối tượng cùng một lúc, dạng còn lại là Single Object Tracking, đó là dạng theo vết duy nhất một đối tượng. Mặc dù cả hai bài toán đều là Tracking nhưng sẽ có nhiều thuật toán và cách xử lý khác nhau để giải quyết.

<section id="2. Nguyên lý">
<b>2. Nguyên lý</b>
</section>
Nguyên lý của bài toán tracking đó là tìm kiếm và so sánh. Ở frame thứ t, ta sẽ tìm kiếm một patch (vùng) và so sánh nó với patch chứa đối tượng ở frame (t-1).

Ở bài này, ta sẽ sử dụng Particle Filter để làm được điều này.

<section id="3. Phương pháp">
<b>3. Phương pháp</b>
</section>
Đối với thuật toán Particle Filter, nó bao gồm 5 bước: <br/>
1) Khởi tạo<br/>
<p align="center">
  <img src="/blog/img/sot/init.png">
</p>

2) Dự đoán<br/>
<p align="center">
  <img src="/blog/img/sot/predict.png">
</p>

<p align="center">
  <img src="/blog/img/sot/predict2.png">
</p>

3)So khớp<br/>
<p align="center">
  <img src="/blog/img/sot/matching.png">
</p>

<p align="center">
  <img src="/blog/img/sot/matching2.png">
</p>

4) Tái lấy mẫu<br/>
<p align="center">
  <img src="/blog/img/sot/resampling.png">
</p>

5) Ước lượng vị trí<br/>
<p align="center">
  <img src="/blog/img/sot/estimate.png">
</p>


<section id="4. Giải thuật">
<b>4. Giải thuật</b>
</section>
<p align="center">
  <img src="/blog/img/sot/algorithm.png">
</p>

<section id="5. Ứng dụng">
<b>5. Ứng dụng</b>
</section>
Đối với bài toán Single Object Tracking, nó được ứng dụng vào các hệ thống theo vết ví dụ, camera hành trình được gắn trên drone để theo vết một ai đó. Hoặc trong các hệ thống surveliance camera, dùng để theo vết đối tượng bằng nhiều camera khác nhau.


<section id="6. Tham khảo">
<b>6. Tham khảo</b>
</section>
Ly Quoc Ngoc, Nguyen Thanh Tin, Le Bao Tuan, International Journal of Advanced Computer Science and Applications (SAI), 14, April, 2020
https://github.com/ngthanhtin/Face_Tracking_ParticleFilter

<div style="text-align: right"> (Tín Nguyễn) </div>