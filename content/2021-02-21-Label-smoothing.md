---
title : "Label Smoothing"
cover : "/blog/img/label_smoothing/label_smoothing.png"
date : "2021-02-21"
tags : 
  - "label smoothing"
# categories : [ "attention" ]

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
Model calibration quan trọng trong việc:
<b>model interpretability and reliability</b><br/>
<b>deciding decision thresholds for downstream applications</b><br/>
<b>integrating our model into an ensemble or a machine learning pipeline</b><br/>
An overconfident model is not calibrated and its predicted probabilities are consistently higher than the accuracy. For example, it may predict 0.9 for inputs where the accuracy is only 0.6. Notice that models with small test errors can still be overconfident, and therefore can benefit from label smoothing.

#### 1.4 Công thức của Label Smoothing
Label smoothing replaces one-hot encoded label vector y_hot with a mixture of y_hot and the uniform distribution:
y_ls = (1 - α) * y_hot + α / K
where K is the number of label classes, and α is a hyperparameter that determines the amount of smoothing. If α = 0, we obtain the original one-hot encoded y_hot. If α = 1, we get the uniform distribution.

#### 1.5 Ý nghĩa đằng sau Label Smoothing
Label smoothing được sử dụng khi loss function là cross-entropy. Đạo hàm của cross entropy loss function đối với logits:<br/>
∇CE = p - y = softmax(z) - y <br/>
trong đó y là label distribution. Cụ thể: <br/>
<b> Gradient descent sẽ làm cho p gần với y.</b><br/>
<b>The gradient giới hạn trong đoạn [-1, 1].</b><br/>

One-hot encoded labels encourages largest possible logit gaps to be fed into the softmax function. Intuitively, large logit gaps combined with the bounded gradient will make the model less adaptive and too confident about its predictions.
In contrast, smoothed labels encourages small logit gaps, as demonstrated by the example below. It is shown in [3] that this results in better model calibration and prevents overconfident predictions.

#### 1.6 Ví dụ
Suppose we have K = 3 classes, and our label belongs to the 1st class. Let [a, b, c] be our logit vector.
If we do not use label smoothing, the label vector is the one-hot encoded vector [1, 0, 0]. Our model will make a ≫ b and a ≫ c. For example, applying softmax to the logit vector [10, 0, 0] gives [0.9999, 0, 0] rounded to 4 decimal places.
If we use label smoothing with α = 0.1, the smoothed label vector ≈ [0.9333, 0.0333, 0.0333]. The logit vector [3.3322, 0, 0] approximates the smoothed label vector to 4 decimal places after softmax, and it has a smaller gap. This is why we call label smoothing a regularization technique as it restrains the largest logit from becoming much bigger than the rest.

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

### 3. Bàn luận
Q: Chọn α như thế nào?<br/>
A: α là một hyperparameter. α = 0.1 thường được sử dụng.<br/>
Q: Có thể dùng một loại distribution khác uniform distribution trong label smoothing được không? <br/>
A: Phần lớn thực nghiệm sử dụng uniform distribution nhưng bạn có thể thử với distrubution khác<br/>

### 4. Tham khảo
https://www.linkedin.com/pulse/label-smoothing-solving-overfitting-overconfidence-code-sobh-phd/?fbclid=IwAR2RWv0x5ZWBfckB9336VVvuHzxrix8p6UXSu1JiKZkAkJGMsBtCsPdDux8 <br/>
https://www.kaggle.com/khyeh0719/pytorch-efficientnet-baseline-train-amp-aug/comments <br/>
https://towardsdatascience.com/what-is-label-smoothing-108debd7ef06

<div style="text-align: right"> (Tín Nguyễn) </div>

