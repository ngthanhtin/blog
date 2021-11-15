---
layout: post
title: Instruction Navigation
subtitle: Navigation using Textual Instruction
gh-repo: daattali/beautiful-jekyll
gh-badge: [settings]
tags: "Reinforcement Learning"
categories: "Reinforcement Learning"
comments: true
cover: "/blog/img/instruction_navigation/example.png"
---
In this post, I want to introduce a hot topic in Reinforcement Learning which is Instruction Navigation. Although Navigation can be tackled by using Supervised Learning, but using Reinforcement Learning help us learn without any human-labeled data.

Link paper: [Paper](https://arxiv.org/abs/1706.07230)<br/>
Link code: [Code](https://github.com/devendrachaplot/DeepRL-Grounding)

The content is as following:<br/>
<a href="#1. Introduction to Instruction Navigation">1. Introduction to Instruction Navigation</a> <br/>
<a href="#2. Principle">2. Principle</a> <br/>
<a href="#3. Methodology">3. Methodology</a> <br/>
* <a href="#3.1 Image Representation Module">3.1 Image Representation Module</a> <br/>
* <a href="#3.2 Text Representation Module">3.2 Text Representation Module</a> <br/>
* <a href="#3.3 Attented Representation Module">3.3 Attented Representation Module</a> <br/>
* <a href="#3.4 Policy Learning">3.4 Policy Learning</a> <br/>

<a href="#4. Application">4. Application</a> <br/>
<a href="#5. Reference">5. Reference</a> <br/>

<section id="1. Introduction to Instruction Navigation">
<b>1. Introduction to Instruction Navigation</b>
</section>
Bài toán Navigation là một bài toán khá quen thuộc, đây là bài toán làm sao để  <b>tác nhân (agent)</b> có thể di chuyển tới một vị trí hoặc một đối tượng nào đó <b>(target)</b>. Thông thường, khi giải quyết bài toán này bằng RL, agent sẽ chỉ nhận được observation là image từ những gì nó thấy được trong environment, image này có thể là RGB image, depth image, segmentation image, etc hoặc observation cũng có thể là image của target.<br/>
Tuy nhiên, ở bài toán này, ngoài image, agent sẽ nhận được input là một câu <b>instruction</b> và agent sẽ tìm cách để đi tới target được đề cập trong câu instruction đó.<br/>
<p align="center">
  <img src="/blog/img/instruction_navigation/instruction_robot_navigation.gif">
</p>

<section id="2. Nguyên lý">
<b>2. Nguyên lý</b>
</section>
Nguyên lí của những bài toán dạng Vision-Language là làm sao để 2 feature vision và language tương tác với nhau.<br/>
Đối với bài toán này, nó sẽ chia thành 3 phần đó là (1) lấy image feature (Embedding), (2) lấy textual feature (embedding) (3) kết hợp 2 feature lại (trong bài này có 2 cách đó là concat 2 features lại hoặc lấy tích Hadarmard (element-wise) của 2 features). Cuối cùng đó là policy learning part, ở bài này họ sử dụng thuật toán Asynchronous Actor Critic (A3C).

<section id="3. Phương pháp">
<b>3. Phương pháp</b>
</section>
<p align="center">
  <img src="/blog/img/instruction_navigation/pp.png">
</p>
Phương pháp đơn giản là input sẽ là 1 image và 1 câu instruction. Chú ý, với mỗi episode thì chỉ có duy nhất 1 câu instruction, qua episode khác thì có câu instruction khác, còn image sẽ thay đổi ở mỗi step.

* <b>3.1 Image Representation Module</b><br/>
Image sẽ được đưa qua một mạng CNN đơn giản để lấy được image embedding

* <b>3.2 Text Representation Module</b><br/>
Instruction thì mỗi từ của câu sẽ được biến thành word embedding. Sau đó biến các word embedding đó thành một sentence emmbeding.

* <b>3.3 Attented Representation Module</b><br/>
<p align="center">
  <img src="/blog/img/instruction_navigation/attention.png">
</p>
After getting a hold of 2 feature vectors, they use Hadardmard (element-wise) product to fuse the features together, and this is called Gated Attention.

* <b>3.4 Policy Learning</b><br/>
<p align="center">
  <img src="/blog/img/instruction_navigation/policy.png">
</p>
The policy learning will help the agent make decision which direction is going on. In this paper, the author use A3C, which is a well-known but simple reinforcement learning algorithm.

<section id="4. Application">
<b>5. Application</b>
</section>
This algorithm can be applied to robotics, however, this stll have some limits such as only handling short, and simple instructions.


<section id="5. Reference">
<b>6. Reference</b>
</section>

[Paper](https://arxiv.org/abs/1706.07230)<br/>
[Code](https://github.com/devendrachaplot/DeepRL-Grounding)

<div style="text-align: right"> (Tín Nguyễn) </div>