---
title : "Label Smoothing"
cover : "/blog/img/label_smoothing/label_smoothing.png"
date : "2021-02-21"
tags : 
  - "label smoothing"
categories : "ML Technique"

---

Hi, xin chào các bạn, hôm này mình sẽ giới thiệu cho các bạn một cách để tránh overfitting và overconfidence đó là phương pháp <b>Label Smoothing</b><br/>

Nội dung chính sẽ bao gồm các phần sau: <br/>

### 1. Label Smoothing là gì?
Đối với một model classification, model thường gặp phải 2 vấn đề: overfitting, and overconfidence. Label smoothing là một phương pháp regularization có thể giải quyết 2 vấn đề kể trên.

#### 1.1 Overfitting là gì?
Overfitting là khi model chỉ tốt trên train data nhưng lại thấp trên validation data. Overfitting có thể được giải quyết bằng nhiều cách khác nhau như là early stopping, dropout, weight regularization, batch normalization.

#### 1.2 Overconfidence là gì?
Overconfidence, là khi model đưa ra kết quả quá tự tin trong khi accuracy không thực sự như vậy. Có rất ít phương pháp để giải quyết vấn đề này.

#### 1.3 Calibration là gì?
A classification model được gọi là calibration khi predicted probabilities của outcomes phản ánh đúng accuracy của model. Ví dụ, ta có 100 mẫu của dataset, mỗi mẫu có predicted probability là 0.9. Nếu model được calibrated, 90 mẫu sẽ được phân loại đúng. 

Một overconfident model là khi predicted probabilities của nó cao hơn accuracy. Ví dụ, model có thể dự đoán xác xuất là 0.9 cho inputs tuy nhiên accuracy chỉ có 0.6.

#### 1.4 Công thức của Label Smoothing
Label smoothing thay thế one-hot encoding label theo phân phối chuẩn như sau:<br/>
y_ls = (1 - α) * y_hot + α / K <br/>
Trong đó, K là số lượng classes và α là một hyperparameter định nghĩa smoothing. Nếu α = 0, công thức quay trở lại thành one-hot encoding. Nếu α = 1, Chúng ta có được phân phối chuẩn uniform.

#### 1.5 Ý nghĩa đằng sau Label Smoothing
Label smoothing được sử dụng khi loss function là cross-entropy. Đạo hàm của cross entropy loss function đối với logits:<br/>
∇CE = p - y = softmax(z) - y <br/>
trong đó y là label distribution. Cụ thể: <br/>
<b> Gradient descent sẽ làm cho p gần với y.</b><br/>
<b>The gradient giới hạn trong đoạn [-1, 1].</b><br/>

One-hot encoded labels cho phép khoảng cách giữa logit sau khi đi qua softmax lớn hơn nhiều so với các logit của các classes còn lại. Và điều này sẽ làm cho model có ít khả năng adaptive và bị over-confident về predictions của nó. <br/>
Ngược lại, smoothed labels sẽ cho ra khoảng cách nhỏ hơn giữa các logit của các classes, giúp giảm thiểu over-confidence.

#### 1.6 Ví dụ
Giả sử có K = 3 classes, và label của mẫu ta đang xét thuộc về 1st class. <br/>
Nếu không dùng label smoothing, label vector sẽ là  one-hot encoded vector [1, 0, 0]. Model của chúng ta sẽ làm cho a ≫ b và a ≫ c. <br/>
Ví dụ, đưa vector này qua softmax sẽ cho ra [0.9999, 0, 0]. Nếu dùng label smoothing với α = 0.1, ta sẽ có smoothed label vector ≈ [0.9333, 0.0333, 0.0333]. Sau khi đưa qua softmax sẽ thành [0.3332, 0, 0], ta có thể thấy khoảng cách giữa logit lớn nhất và các phần tử còn lại nhỏ hơn ban đầu. <br/>
Đó là lí do tại sao gọi label smoothing là một kĩ thuật regularization vì nó giới hạn lại phần tử logit lớn nhất không bị trở nên quá lớn hơn các phần tử còn lại. <br/>

### 2. Code
Tensorflow: Label smoothing đã được định nghĩa trong Tensorflow. Các bạn có thể xem source của BinaryCrossentropy CategoricalCrossentropy để biết thêm chi tiết.<br/>
PyTorch: Mình vẫn chưa thấy Pytorch có định nghĩa sẵn hàm này, các bạn có thể tham khảo đoạn code sau.<br/>
```
class MyCrossEntropyLoss(_WeightedLoss):
    def __init__(self, weight=None, reduction='mean'):
        super().__init__(weight=weight, reduction=reduction)
        self.weight = weight
        self.reduction = reduction

    def forward(self, inputs, targets):
        lsm = F.log_softmax(inputs, -1)

        if self.weight is not None:
            lsm = lsm * self.weight.unsqueeze(0)

        loss = -(targets * lsm).sum(-1)

        if  self.reduction == 'sum':
            loss = loss.sum()
        elif  self.reduction == 'mean':
            loss = loss.mean()

        return loss
```
Hoặc các bạn có thể tham khảo đoạn code [link](https://colab.research.google.com/drive/1HiZIS4G8QaOJNc0xARnMUT2LExocpQI3#scrollTo=z7ptOPY3oJzR)
### 3. Bàn luận
Q: Chọn α như thế nào?<br/>
A: α là một hyperparameter. α = 0.1 thường được sử dụng.<br/>
Q: Có thể dùng một loại distribution khác uniform distribution trong label smoothing được không? <br/>
A: Phần lớn thực nghiệm sử dụng uniform distribution nhưng bạn có thể thử với distrubution khác<br/>

### 4. Tham khảo
https://www.linkedin.com/pulse/label-smoothing-solving-overfitting-overconfidence-code-sobh-phd/?fbclid=IwAR2RWv0x5ZWBfckB9336VVvuHzxrix8p6UXSu1JiKZkAkJGMsBtCsPdDux8 <br/>
https://www.kaggle.com/khyeh0719/pytorch-efficientnet-baseline-train-amp-aug/comments <br/>
https://towardsdatascience.com/what-is-label-smoothing-108debd7ef06<br/>
https://www.kaggle.com/c/siim-isic-melanoma-classification/discussion/173733<br/>
<div style="text-align: right"> (Tín Nguyễn) </div>

