# [AdvDiffuser: Natural Adversarial Example Synthesis with Diffusion Models](https://openaccess.thecvf.com/content/ICCV2023/html/Chen_AdvDiffuser_Natural_Adversarial_Example_Synthesis_with_Diffusion_Models_ICCV_2023_paper.html)

总结：略。

## 3.Method
其方法包含两个部分：Adversarial guidance（对抗性引导）和Adversarial inpainting（对抗性修补）。给定一个原图 **x** ，先求解其相对于目标分类器 $f$ 的Grad-CAM以得到一个Mask，然后将其反转得到Mask-Inv.，两者分别控制着重添加扰动的部分和其他部分。在Diffusion的反向过程中， $t$ 时刻的噪声样本 $x_t$ 经去噪网络 $p\_{\theta}$ 处理后得到 $\widetilde{x}\_{t-1}$ , 然后将其放入目标分类器 $f$ 使用PGD算法得到扰动。使用Mask和Mask-Inv.来分别控制图像主体和背景的扰动强度：

$$ x\_{t-1} = m \odot x\^{obj}\_{t-1} + (1-m) \odot \hat{x}\_{t-1} $$ 

其中

$$ x\^{obj}\_{t-1} \thicksim \mathcal{N} (\sqrt{\overline{\alpha}\_{t}} \textbf{x}\_{0}, (1 - \overline{\alpha}\_{t} )\textbf{I} ), $$

$$ \hat{x}\_{t-1} = PGD( \widetilde{x}\_{t-1}, f, \sigma \beta\_{t-1}, I ), $$

$$ \widetilde{x}\_{t-1} \thicksim p\_{\theta}( \textbf{x}\_{t-1} | \textbf{x}\_t ) $$
