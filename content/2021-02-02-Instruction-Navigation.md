---
layout: post
title: Instruction Navigation
subtitle: Navigation using Textual Instruction
gh-repo: daattali/beautiful-jekyll
gh-badge: [settings]
tags: [rl, navigation, attention, nlp]
comments: true
cover: "/blog/img/instruction_navigation/example.png"
---
Hi, xin chào các bạn, hôm này mình sẽ giới thiệu cho các bạn bài toán <b>Instruction Navigation</b> sử dụng Reinforcement Learning (có nhiều cách để giải bài toán này, chúng ta cũng có thể sử dụng Supervised Learning để giải).<br/>
Link paper: [Paper](https://arxiv.org/abs/1706.07230)<br/>
Link code: [Code](https://github.com/devendrachaplot/DeepRL-Grounding)

Nội dung chính sẽ bao gồm các phần sau:<br/>
<a href="#1. Giới thiệu bài toán Instruction Navigation">1. Giới thiệu bài toàn Instruction Navigation</a> <br/>
<a href="#2. Nguyên lý">2. Nguyên lý</a> <br/>
<a href="#3. Phương pháp">3. Phương pháp</a> <br/>
* <a href="#3.1 Image Representation Module">3.1 Image Representation Module</a> <br/>
* <a href="#3.2 Text Representation Module">3.2 Text Representation Module</a> <br/>
* <a href="#3.3 Attented Representation Module">3.3 Attented Representation Module</a> <br/>
* <a href="#3.4 Policy Learning">3.4 Policy Learning</a> <br/>

<a href="#4. Ứng dụng">4. Ứng dụng</a> <br/>
<a href="#5. Tham khảo">5. Tham khảo</a> <br/>

<section id="1. Giới thiệu bài toán Instruction Navigation">
<b>1. Giới thiệu bài toán Multiple Object Tracking</b>
</section>
Bài toán Navigation là một bài toán khá quen thuộc, đây là bài toán làm sao để  <b>tác nhân (agent)</b> có thể di chuyển tới một vị trí hoặc một đối tượng nào đó <b>(target)</b>. Thông thường, khi giải quyết bài toán này bằng RL, agent sẽ chỉ nhận được observation là image từ những gì nó thấy được trong environment, image này có thể là RGB image, depth image, segmentation image, etc hoặc observation cũng có thể là image của target.<br/>
Tuy nhiên, ở bài toán này, ngoài image, agent sẽ nhận được input là một câu <b>instruction</b> và agent sẽ tìm cách để đi tới target được đề cập trong câu instruction đó.<br/>
<p align="center">
  <img src="/blog/img/instruction_navigation/instruction_robot_navigation.gif">
</p>

<section id="2. Nguyên lý">
<b>2. Nguyên lý</b>
</section>


<section id="3. Phương pháp">
<b>3. Phương pháp</b>
</section>

* <b>3.1 Image Representation Module</b><br/>

* <b>3.2 Text Representation Module</b><br/>

* <b>3.3 Attented Representation Module</b><br/>

* <b>3.4 Policy Learning</b><br/>

<section id="4. Ứng dụng">
<b>5. Ứng dụng</b>
</section>


<section id="5. Tham khảo">
<b>6. Tham khảo</b>
</section>
[Code](https://github.com/devendrachaplot/DeepRL-Grounding)<br/>

<div style="text-align: right"> (Tín Nguyễn) </div>