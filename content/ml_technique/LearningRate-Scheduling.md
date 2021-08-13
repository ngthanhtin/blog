---
title : "Learning Rate Scheduling in Pytorch"
cover : "/blog/img/learningrate_scheduling/cyclicalLR_triangular.png"
date : "2021-03-25"
tags : 
  - "machine-leanring"
  - "learning-rate"
  - "scheduling"
categories : "ML Technique"
---

Hi, bài viết hôm nay mình sẽ nói qua một số phương pháp điều chỉnh learning rate trong quá trình train một model hay còn gọi là <b>learning rate scheduler</b>. Việc điều chỉnh learning rate trong quá trình train là một việc rất có lợi để model có thể tìm đc local minima một cách hợp lí hơn.

Nội dung chính sẽ bao gồm các phần sau: <br/>

1. Lambda LR.
2. Multiplicative LR.
3. Step LR.
4. MultiStep LR.
5. Exponential LR.
6. CosineAnnealing LR.
7. Cyclic LR.
8. OneCycle LR.
9. Cosine Annealing Warm Restarts.



### 1. Lambda LR.

<p align="center">
<img src="https://render.githubusercontent.com/render/math?math=lr_{epoch} = lr_{initial} * lambda_{epoch}">
</p>

```
model = torch.nn.Linear(2, 1)
optimizer = torch.optim.SGD(model.parameters(), lr=100)
lambda1 = lambda epoch: 0.65 ** epoch
scheduler = torch.optim.lr_scheduler.LambdaLR(optimizer, lr_lambda=lambda1)


lrs = []

for i in range(10):
    optimizer.step()
    lrs.append(optimizer.param_groups[0]["lr"])
    scheduler.step()

plt.plot(range(10),lrs)
```

<p align="center">
  <img src="/blog/img/learningrate_scheduling/lambdaLR.png">
</p>

### 2. Multiplicative LR.
<p align="center">
<img src="https://render.githubusercontent.com/render/math?math=lr_{epoch} = lr_{epoch-1} * lambda_{epoch}">
</p>

```
model = torch.nn.Linear(2, 1)
optimizer = torch.optim.SGD(model.parameters(), lr=100)
lmbda = lambda epoch: 0.65 ** epoch
scheduler = torch.optim.lr_scheduler.MultiplicativeLR(optimizer, lr_lambda=lmbda)
lrs = []

for i in range(10):
    optimizer.step()
    lrs.append(optimizer.param_groups[0]["lr"])
#     print("Factor = ",0.95," , Learning Rate = ",optimizer.param_groups[0]["lr"])
    scheduler.step()

plt.plot(range(10),lrs)
```

<p align="center">
  <img src="/blog/img/learningrate_scheduling/multiplicativeLR.png">
</p>

### 3. Step LR.
Giảm learning rate theo gamma ở mỗi step size của mỗi epoch.<br/>
<p align="center">
<img src="https://render.githubusercontent.com/render/math?math=lr_{epoch} =  \begin{cases}
                  \gamma * lr_{epoch-1} \\
                  lr_{epoch-1}
                  \end{cases}">
</p>

```
model = torch.nn.Linear(2, 1)
optimizer = torch.optim.SGD(model.parameters(), lr=100)
scheduler = torch.optim.lr_scheduler.StepLR(optimizer, step_size=2, gamma=0.1)
lrs = []

for i in range(10):
    optimizer.step()
    lrs.append(optimizer.param_groups[0]["lr"])
#     print("Factor = ",0.1 if i!=0 and i%2!=0 else 1," , Learning Rate = ",optimizer.param_groups[0]["lr"])
    scheduler.step()

plt.plot(range(10),lrs)
```

<p align="center">
  <img src="/blog/img/learningrate_scheduling/stepLR.png">
</p>

### 4. MultiStep LR.
Giảm learning rate theo gamma mỗi khi số lượng epoch đạt tới một giá trị trong milestones (<img src="https://render.githubusercontent.com/render/math?math=lr_{epoch} \in milestones">)

<p align="center">
<img src="https://render.githubusercontent.com/render/math?math=lr_{epoch} =  \begin{cases}
                  \gamma * lr_{epoch-1} \\
                  lr_{epoch-1}
                  \end{cases}">
</p>

```
model = torch.nn.Linear(2, 1)
optimizer = torch.optim.SGD(model.parameters(), lr=100)
scheduler = torch.optim.lr_scheduler.MultiStepLR(optimizer, milestones=[6,8,9], gamma=0.1)
lrs = []

for i in range(10):
    optimizer.step()
    lrs.append(optimizer.param_groups[0]["lr"])
#     print("Factor = ",0.1 if i in [6,8,9] else 1," , Learning Rate = ",optimizer.param_groups[0]["lr"])
    scheduler.step()

plt.plot(range(10),lrs)
```

<p align="center">
  <img src="/blog/img/learningrate_scheduling/multistepLR.png">
</p>

### 5. Exponential LR.
<p align="center">
<img src="https://render.githubusercontent.com/render/math?math=lr_{epoch} = lr_{epoch-1} * \gamma">
</p>

```
model = torch.nn.Linear(2, 1)
optimizer = torch.optim.SGD(model.parameters(), lr=100)
scheduler = torch.optim.lr_scheduler.ExponentialLR(optimizer, gamma=0.1)
lrs = []


for i in range(10):
    optimizer.step()
    lrs.append(optimizer.param_groups[0]["lr"])
#     print("Factor = ",0.1," , Learning Rate = ",optimizer.param_groups[0]["lr"])
    scheduler.step()

plt.plot(lrs)
```

<p align="center">
  <img src="/blog/img/learningrate_scheduling/exponentialLR.png">
</p>

### 6. CosineAnnealing LR.
Được đề xuất trong bài SGDR: Stochastic Gradient Descent with Warm Restarts. Link: https://arxiv.org/abs/1608.03983

<p align="center">
<img src="https://render.githubusercontent.com/render/math?math=\eta_{t}_ = \eta_{min} %2B \frac{1}{2}(\eta_{max} - \eta_{min})(1 - cos(\frac{T_{cur}}{T_{max}}\pi))">
</p>

```
model = torch.nn.Linear(2, 1)
optimizer = torch.optim.SGD(model.parameters(), lr=100)
scheduler = torch.optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max=10, eta_min=0)
lrs = []


for i in range(100):
    optimizer.step()
    lrs.append(optimizer.param_groups[0]["lr"])
#     print("Factor = ",i," , Learning Rate = ",optimizer.param_groups[0]["lr"])
    scheduler.step()

plt.plot(lrs)
```

<p align="center">
  <img src="/blog/img/learningrate_scheduling/cosineanealingLR.png">
</p>

### 7. Cyclic LR.
#### 7.1 Triangular.

```
model = torch.nn.Linear(2, 1)
optimizer = torch.optim.SGD(model.parameters(), lr=0.1)
scheduler = torch.optim.lr_scheduler.CyclicLR(optimizer, base_lr=0.001, max_lr=0.1,step_size_up=5,mode="triangular")
lrs = []


for i in range(100):
    optimizer.step()
    lrs.append(optimizer.param_groups[0]["lr"])
#     print("Factor = ",i," , Learning Rate = ",optimizer.param_groups[0]["lr"])
    scheduler.step()

plt.plot(lrs)
```

<p align="center">
  <img src="/blog/img/learningrate_scheduling/cyclicalLR_triangular.png">
</p>

#### 7.2 Triangular2.

```
model = torch.nn.Linear(2, 1)
optimizer = torch.optim.SGD(model.parameters(), lr=0.1)
scheduler = torch.optim.lr_scheduler.CyclicLR(optimizer, base_lr=0.001, max_lr=0.1,step_size_up=5,mode="triangular2")
lrs = []


for i in range(100):
    optimizer.step()
    lrs.append(optimizer.param_groups[0]["lr"])
#     print("Factor = ",i," , Learning Rate = ",optimizer.param_groups[0]["lr"])
    scheduler.step()

plt.plot(lrs)
```

<p align="center">
  <img src="/blog/img/learningrate_scheduling/cyclicalLR_triangular2.png">
</p>

#### 7.3 Exp_range.

```
model = torch.nn.Linear(2, 1)
optimizer = torch.optim.SGD(model.parameters(), lr=100)
scheduler = torch.optim.lr_scheduler.CyclicLR(optimizer, base_lr=0.001, max_lr=0.1,step_size_up=5,mode="exp_range",gamma=0.85)
lrs = []


for i in range(100):
    optimizer.step()
    lrs.append(optimizer.param_groups[0]["lr"])
#     print("Factor = ",i," , Learning Rate = ",optimizer.param_groups[0]["lr"])
    scheduler.step()

plt.plot(lrs)
```

<p align="center">
  <img src="/blog/img/learningrate_scheduling/cyclicalLR_exp.png">
</p>

### 8. OneCycle LR.
Learning rate được thay đổi theo phương pháp 1cycle learning rate policy. 1cycle policy tăng learning rate từ giá trị ban đầu tới một giá trị maximum nào đó sau đó giảm xuống một giá trị minimum còn nhỏ hơn giá trị learning rate ban đầu.<br/> 
Phương pháp này được đề xuất đầu tiên ở paper Super-Convergence: Very Fast Training of Neural Networks Using Large Learning Rates.<br/>
The 1cycle learning rate policy thay đổi the learning rate sau mỗi batch.<br/>
#### 8.1 Cos.
```
model = torch.nn.Linear(2, 1)
optimizer = torch.optim.SGD(model.parameters(), lr=0.1)
scheduler = torch.optim.lr_scheduler.OneCycleLR(optimizer, max_lr=0.1, steps_per_epoch=10, epochs=10)
lrs = []


for i in range(100):
    optimizer.step()
    lrs.append(optimizer.param_groups[0]["lr"])
#     print("Factor = ",i," , Learning Rate = ",optimizer.param_groups[0]["lr"])
    scheduler.step()

plt.plot(lrs)
```

<p align="center">
  <img src="/blog/img/learningrate_scheduling/onecycleLR_cos.png">
</p>

#### 8.1 Linear.

```
model = torch.nn.Linear(2, 1)
optimizer = torch.optim.SGD(model.parameters(), lr=0.1)
scheduler = torch.optim.lr_scheduler.OneCycleLR(optimizer, max_lr=0.1, steps_per_epoch=10, epochs=10,anneal_strategy='linear')
lrs = []


for i in range(100):
    optimizer.step()
    lrs.append(optimizer.param_groups[0]["lr"])
#     print("Factor = ",i," , Learning Rate = ",optimizer.param_groups[0]["lr"])
    scheduler.step()

plt.plot(lrs)
```

<p align="center">
  <img src="/blog/img/learningrate_scheduling/onecycleLR_linear.png">
</p>

### 9. Cosine Annealing Warm Restarts.
Đặt learning rate sử dụng một cosine annealing schedule, và restarts lại sau Ti epochs.<br/>
<p align="center">
<img src="https://render.githubusercontent.com/render/math?math=\eta_{t}_ = \eta_{min} %2B \frac{1}{2}(\eta_{max} - \eta_{min})(1 - cos(\frac{T_{cur}}{T_{i}}\pi))">
</p>

```
import torch
import matplotlib.pyplot as plt

model = torch.nn.Linear(2, 1)
optimizer = torch.optim.SGD(model.parameters(), lr=0.1)
lr_sched = torch.optim.lr_scheduler.CosineAnnealingWarmRestarts(optimizer, T_0=10, T_mult=1, eta_min=0.001, last_epoch=-1)


lrs = []

for i in range(100):
    lr_sched.step()
    lrs.append(
        optimizer.param_groups[0]["lr"]
    )

plt.plot(lrs)
```

<p align="center">
  <img src="/blog/img/learningrate_scheduling/cosineanealing_warmrestartLR.png">
</p>

```
import torch
import matplotlib.pyplot as plt

model = torch.nn.Linear(2, 1)
optimizer = torch.optim.SGD(model.parameters(), lr=0.1)
lr_sched = torch.optim.lr_scheduler.CosineAnnealingWarmRestarts(optimizer, T_0=10, T_mult=2, eta_min=0.01, last_epoch=-1)


lrs = []

for i in range(300):
    lr_sched.step()
    lrs.append(
        optimizer.param_groups[0]["lr"]
    )

plt.plot(lrs)
```

<p align="center">
  <img src="/blog/img/learningrate_scheduling/cosineanealing_warmrestartLR2.png">
</p>

<div style="text-align: right"> (Tín Nguyễn) </div>
