# [SemanticAdv: Generating Adversarial Examples via Attribute-conditioned Image Editing](https://www.ecva.net/papers/eccv_2020/papers_ECCV/papers/123590018.pdf)

## [Github repository](https://github.com/AI-secure/SemanticAdv)

## 总结
通过对原始图片进行语义层面的编辑实现攻击目的。

## 方法
给定一张原始图片，首先使用一个属性控制的(attribute-conditioned)图像编辑模型(文中使用的是StarGAN)对其进行语义编辑，得到一张与原图非常接近、只在某一个语义维度上有区别的合成图像。然后在特征空间中对原图和编辑后的图像的特征图做插值即可得到一张介于原图和合成图像之间的对抗样本。

## 实验
在一个人脸识别数据集和CityScape数据集上分别进行了分类和分割任务的攻击，局限性比较大，似乎StarGAN在人脸上的表现比较好。分类攻击效果较好，分割攻击效果不太明显，且给出的例子也有限。
