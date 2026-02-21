# Base-unet视网膜血管分割对比实验：Base-Unet / Retina-Unet / Attention-Unet
  本毕设针对眼底血管分割任务，实现并对比了三种基于 U-Net 的改进模型，研究不同模型在细微血管分割精度方面的效果，展示学习成果与实践探索。
  
## 项目结构
    ├── 01_Base-Unet/          # 基础 U-Net 模型（基线）
    ├── 02_Retina-Unet/    # 纯 Retina-Unet 模型（多尺度融合）
    ├── 03_Attention-Unet/     # 带软注意力门控的 Attention-Unet（最终改进）
    ├── DRIVE/                 # DRIVE 数据集（训练/测试图像与金标准）
    ├── lib/                   # 公共工具库（数据提取、可视化、辅助函数）
    └── README.md              # 项目说明文档
    
## 数据集
  • 数据集：DRIVE（Digital Retinal Images for Vessel Extraction）
  
  • 训练集：20 张眼底图像 + 对应血管标注
  
  • 测试集：20 张眼底图像 + 对应血管标注
  
  • 预处理：图像归一化至 0–1，提取视野内（FOV）的 48×48 补丁进行训练
  

## 模型简介
  模型           |核心改进                                |设计目标                                  | 
  ---------------|----------------------------------------|-----------------------------------------|
  Base-Unet      |标准U-Net结构:编码器——解码器+跳跃连接     |作为基线模型，提供基础分割能力              |
  Retina-Unet    |多尺度特征融合，增强对细血管的捕捉         |提升小血管检测能力                         |
  Attention-Unet |在 Retina-Unet 基础上加入软注意力门控     |聚焦血管区域，抑制背景干扰，进一步提升分割精度|

## 训练配置
  • 训练轮数：100 轮
  
  • 批次大小：32
  
  • 补丁尺寸：48×48
  
  • 优化器：SGD
  
  • 损失函数：交叉熵（Dice Loss 可选）
  
  • 验证集：训练集的 10% 作为验证集
  

## 预期对比指标

  模型           |Dice 系数  |IoU 灵敏度|特异度| 
  ---------------|------------|---------|-------|
  Base-Unet      |0.78       |0.68      |0.75 |<br>
  Retina-Unet    |0.81       |0.71      |0.78 |<br>
  Attention-Unet |0.84       |0.74      |0.80 |<br>

# 快速开始
  ## 1. 环境依赖
     环境依赖为Python3.7.9，
     keras == 2.3.1
     TensorFlow == 1.15.0
     Numpy ==1.18.0
     h5py == 2.10.0
     OpenCV-Python
     Matplotlib
     
  ## 2. 训练模型
    进入对应模型目录，运行训练脚本：
      训练 Base-Unet
        cd 01_Base-Unet
        python train.py
      
      训练 Retina-Unet
        cd ../02_Retina-Unet
        python train.py
      
      训练 Attention-Unet
        cd ../03_Attention-Unet
        python train.py
    
  ## 3. 测试与可视化
    使用训练好的最优权重（*_best_weights.h5）进行测试，并可视化分割结果。
  
  # 项目亮点
  • 严格控制变量，三种模型在相同数据、相同训练条件下对比，结果公平可靠
  
  • 从基础 U-Net 到注意力增强模型，清晰展示了医学图像分割的改进路径
  
  • 代码结构清晰，可直接复用或扩展到其他医学分割任务
