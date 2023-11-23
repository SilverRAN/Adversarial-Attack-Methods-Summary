# [Semantic Adversarial Attacks: Parametric Transformations That Fool Deep Classifiers](https://arxiv.org/abs/1904.08489)

## 总结：
**Semantic-attack / White-box / GAN**

本文提出了一种针对特殊输入进行语义攻击的方法。对于一类具有显著的、不变的特征以及可变属性的输入图像(例如人脸，其中肤色、比例、五官都是显著且不变的特征，而发色、是否带眼镜都是可变的属性，改变可变属性并不影响对图像整体的识别), 改变其中一部分属性同样可以产生对抗样本。尽管这些样本与原图的差别很大，但在人眼看来它与原图确实是属于同一个人，同时它又能误导分类器。这种形式的攻击更有现实意义，因为我们需要人脸识别系统不受发色、眼镜或口罩的影响，以及自动驾驶系统不受天气、路面状况、光线条件的影响。但文章的实验结果很有限，仅仅统计了对一个通过人脸进行性别识别的二分类模型进行攻击的效果(作者称他们的框架transparently extends to multi-class models)。此外，这篇文章之所以称为"parametric transformation"或者说是"parametric space"的攻击，是因为其需要更改的视觉特征需要先参数化为一个输入向量，因此其编辑空间是一个参数空间。

## 方法
给定一张输入图片 $\text{x}\_{0}$ 以及一个attribute vector $\text{a}\_{0}$ , 首先使用一个现成的Attribute encoder $E(\cdot)$ 将其编码至参数空间得到 $\overline{\text{a}}\_{0}$ ，然后输入现成的参数生成器 $G(\cdot, \cdot)$ 以生成编辑后的样本 $\text{x}\_{i}$ , 并输入要攻击的分类器 $f(\cdot)$ 得到当下的预测类别 $h\_{i}$ 。若 $h\_{i}=\text{y}$, 计算其与真实类别 $\text{y}$ 之间的loss $l\_{adv}$ ，通过该loss进行梯度反传以更新attribute vector并得到 $\overline{\text{a}}\_{i}$，重复上述步骤直到预测类别不等于真实类别 $h\_{i} \not= \text{y}$ .

具体使用的模型是Fader Networks和Attribute GANs。
