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

## Overfitting là gì?
Overfitting là khi model chỉ tốt trên train data nhưng lại thấp trên validation data. Overfitting có thể được giải quyết bằng nhiều cách khác nhau như là early stopping, dropout, weight regularization, batch normalization.

## Overconfidence là gì?
Overconfidence, là khi model đưa ra kết quả quá tự tin trong khi accuracy không thực sự như vậy. Có rất ít phương pháp để giải quyết vấn đề này.

## Calibration là gì?
A classification model được gọi là calibration khi predicted probabilities của outcomes phản ánh đúng accuracy của model. Ví dụ, ta có 100 mẫu của dataset, mỗi mẫu có predicted probability là 0.9. Nếu model được calibrated, 90 mẫu sẽ được phân loại đúng. 
Model calibration quan trọng trong việc:
# model interpretability and reliability
# deciding decision thresholds for downstream applications
# integrating our model into an ensemble or a machine learning pipeline
An overconfident model is not calibrated and its predicted probabilities are consistently higher than the accuracy. For example, it may predict 0.9 for inputs where the accuracy is only 0.6. Notice that models with small test errors can still be overconfident, and therefore can benefit from label smoothing.

## Formula of Label Smoothing
Label smoothing replaces one-hot encoded label vector y_hot with a mixture of y_hot and the uniform distribution:
y_ls = (1 - α) * y_hot + α / K
where K is the number of label classes, and α is a hyperparameter that determines the amount of smoothing. If α = 0, we obtain the original one-hot encoded y_hot. If α = 1, we get the uniform distribution.

## Motivation of Label Smoothing
Label smoothing is used when the loss function is cross entropy, and the model applies the softmax function to the penultimate layer’s logit vectors z to compute its output probabilities p. In this setting, the gradient of the cross entropy loss function with respect to the logits is simply
∇CE = p - y = softmax(z) - y
where y is the label distribution. In particular, we can see that
# Gradient descent will try to make p as close to y as possible.
# The gradient is bounded between -1 and 1.
One-hot encoded labels encourages largest possible logit gaps to be fed into the softmax function. Intuitively, large logit gaps combined with the bounded gradient will make the model less adaptive and too confident about its predictions.
In contrast, smoothed labels encourages small logit gaps, as demonstrated by the example below. It is shown in [3] that this results in better model calibration and prevents overconfident predictions.

## A Concrete Example
Suppose we have K = 3 classes, and our label belongs to the 1st class. Let [a, b, c] be our logit vector.
If we do not use label smoothing, the label vector is the one-hot encoded vector [1, 0, 0]. Our model will make a ≫ b and a ≫ c. For example, applying softmax to the logit vector [10, 0, 0] gives [0.9999, 0, 0] rounded to 4 decimal places.
If we use label smoothing with α = 0.1, the smoothed label vector ≈ [0.9333, 0.0333, 0.0333]. The logit vector [3.3322, 0, 0] approximates the smoothed label vector to 4 decimal places after softmax, and it has a smaller gap. This is why we call label smoothing a regularization technique as it restrains the largest logit from becoming much bigger than the rest.
### 2. Label smoothing

### 3. Code
Implementation
Tensorflow: Label smoothing đã được định nghĩa trong Tensorflow. Các bạn có thể xem source của BinaryCrossentropy CategoricalCrossentropy để biết thêm chi tiết.
PyTorch: Mình vẫn chưa thấy Pytorch có định nghĩa sẵn hàm này, các bạn có thể tham khảo đoạn code sau.

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

### 4. Tham khảo
https://www.linkedin.com/pulse/label-smoothing-solving-overfitting-overconfidence-code-sobh-phd/?fbclid=IwAR2RWv0x5ZWBfckB9336VVvuHzxrix8p6UXSu1JiKZkAkJGMsBtCsPdDux8
https://www.kaggle.com/khyeh0719/pytorch-efficientnet-baseline-train-amp-aug/comments
https://towardsdatascience.com/what-is-label-smoothing-108debd7ef06

<div style="text-align: right"> (Tín Nguyễn) </div>


Frequently Asked Questions
Q: When do we use label smoothing?
A: Whenever a classification neural network suffers from overfitting and/or overconfidence, we can try label smoothing.
Q: How do we choose α?
A: Just like other regularization hyperparameters, there is no formula for choosing α. It is usually done by trial and error, and α = 0.1 is a good place to start.
Q: Can we use distributions other than uniform distribution in label smoothing?
A: Technically yes. In [4] the theoretical groundwork is developed for arbitrary distributions. That being said, the vast majority of empirical studies on label smoothing use uniform distribution.
Q: Is label smoothing used outside deep learning?
A: Not really. Most popular non-deep learning methods do not use the softmax function. Thus label smoothing is usually not applicable.