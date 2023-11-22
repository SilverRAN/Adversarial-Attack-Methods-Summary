# [Unrestricted Adversarial Examples via Semantic Manipulation](https://arxiv.org/abs/1904.06347)

## 总结
通过语义操作，即改变输入图像的color和texture来实现攻击效果。虽然公开了代码，但没有提供ckpt和数据文件，而且代码非常混乱，无法复现。

## 方法
### 着色攻击
使用了一个现有的实时着色模型(Real-Time User-Guided Image Colorization with Learned Deep Priors), 通过用户点击来改变原图中的灰色区域。
### 纹理攻击
详细讲述一下这部分。纹理迁移(Text Transfer)任务可以认为是风格迁徙(Style Transfer)的相似任务。据作者解释，他们所采用的方法是最小化两张图像的层间格拉姆矩阵(cross-layer gram matrices)的距离。优化 $\mathcal{Loss}$ 由两部分组成：

$$ L\^{\mathcal{A}}\_{\text{tAdv}} = \alpha L\_{t}\^{\mathcal{A}} (I\_{v}, I\_{t}) + \beta J\_{adv}(\mathcal{F}(I\_{v}), t) $$

其中 $L\_{t}\^{\mathcal{A}}$ 表示纹理迁移损失(texture transfer loss), $J\_{adv}$ 是一个cross-entropy损失，该损失中的 $t$ 似乎是提供纹理的源图像的类别，作者在文中提到该损失可以帮助优化过程。

## 实验
在ImageNet上随机选择了10个差异较大的class的图像进行了分类攻击测试，并且在MSCOCO数据集上随机选择图像进行了Image Captioning攻击测试。
