---
layout: post
title: Instruction Navigation
subtitle: Navigation using Textual Instruction
gh-repo: daattali/beautiful-jekyll
gh-badge: [settings]
tags: ["Reinforcement Learning", "Vision Language"]
categories: ["Reinforcement Learning", Vision Language"]
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
Navigation problem is a familiar topic and it has been carried out many experiments in so many years. The problem is to make an agent move in an environment and achieve a predefined target. In general, this problem can be solved by Reinforcement Learning, the agent will receive an observation which contains several information such as image, depth, segmentation or sensor information, etc. Howerver, human want to make command for the agent through instruction, so the agent now has an additional information which is a text, or voice. The low-level instruction will be simple commands such as go straight, go left, go right, etc, but in practice, the instruction is more complex, it needs to consider other elements such as size, color, etc of the objects nearby. And that is the motivation of the Instruction Navigation problem.
<p align="center">
  <img src="/blog/img/instruction_navigation/instruction_robot_navigation.gif">
</p>

<section id="2. Principle">
<b>2. Principle</b>
</section>
The principle of Vision-Language problems is how to fuse multiple features together. <br/>
And with this problem, it composes of 3 components, (1) is to get image features, (2) is to get textual features, (3) is how to fuse these features together which is the most important part. In this paper, the fusion strategy they've used is concatenation and Hadarmard (element-wise) product. The final stage is the decision-making part, which employs a policy learning algorithm to make decision. A well-known but simple algorithm used in this situation is Asynchronous Actor Critic (A3C).

<section id="3. Methodology">
<b>3. Methodology</b>
</section>
<p align="center">
  <img src="/blog/img/instruction_navigation/pp.png">
</p>
The methodology simply takes an image and an instruction as inputs. Noted that, with each episode, there is only one instruction, but the image changes continually.

* <b>3.1 Image Representation Module</b><br/>
In order to extract visual features, they employed a simple 3-layer CNN.

* <b>3.2 Text Representation Module</b><br/>
In terms of extracting textual features, first, they employed Word Embedding on the text input, then converted it to sentence embedding by using LSTM or GRU.

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