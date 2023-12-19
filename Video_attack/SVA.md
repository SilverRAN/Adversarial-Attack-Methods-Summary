# Sparse Black-Box Video Attack with Reinforcement Learning

## Abstract
传统的视频攻击方法要么是将每一帧视为平等的进行攻击，要么是使用一些独立于攻击手段之外的策略选出关键帧来攻击。而本文作者提出，关键帧选择应该是一个与攻击手段紧密相关的过程。因此，本文将黑盒视频攻击嵌入到RL框架中，将视频识别model视为RL的environment，由RL的agent来选取帧。通过对两个视频分类模型（C3D和LRCN）进行测试，证明了方法的有效性。

