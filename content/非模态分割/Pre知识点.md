
1. 非显现补全（Amodal Completion）：是指人类能够在观察到部分遮挡的物体时，根据已有的视觉信息来推测和补全被遮挡部分的整体形状和结构的能力。它与显现（Modal）补全相对立：显现补全是指对可见部分的直接重建，而非显现补全则是在看不到的情况下“想象”出完整物体的能力。
      **image amodal completion** 是指在数字图像中，使用算法来重建被遮挡的物体或形状，使得计算机能够“推测”出遮挡背后的部分。这种补全并不仅仅依赖于当前可见的图像信息，还需要结合对物体形状、结构和语义信息的理解。
       - **基于几何推测的方法（Geometry-based Methods）**：
        - 传统的几何推测方法主要依赖边缘检测、形状先验（如物体的对称性、共线性）等几何特征来推测被遮挡部分的形状。
       - **基于深度学习的方法（Deep Learning-based Methods）**：
		   - 随着深度学习的发展，基于卷积神经网络（CNN）和生成对抗网络（GAN）的模型能够从部分遮挡的图像中学习潜在的形状分布，从而生成被遮挡部分的图像内容。
		   - 此类模型通常通过大规模数据集进行训练，学会物体的各种形状、轮廓和语义信息，从而能够推测遮挡部分的内容。典型方法包括 Mask R-CNN（用于物体分割和形状补全）以及各种基于 Transformer 的补全模型。
     - **结合语义信息的方法（Semantic-aware Methods）**：
        - 非显现补全不仅需要考虑图像的几何形状，还需要理解物体的语义属性（如“人”的形状与“车”的形状显著不同）。基于语义分割或语义理解的补全模型能够通过识别物体类别来推测被遮挡部分的形状。

	它的三个任务：
	amodal shape completion, 
	amodal appearance completion, 
	order perception.

 - 近年来的一些方法：
  近年来，随着深度学习技术的发展，基于深度学习的非模态补全方法已经取得了很大的进展。通过学习大量数据中的统计规律，深度学习模型可以自动推断出缺失的信息，从而实现对非模态补全任务的有效处理。Zhan 等人提出的 PCNet(Partial CompletionNetwork)[1] 方法，使用两个独立的网络：PCNet-M，PCNet-C 顺序地实现遮挡关系推理、非模态形状补全和非模态外观补全。PCNet 通过自监督的方式进行训练，其框架不需要非模态分割标注或任何类型的 3D 合成数据的注释，其方法能够解决高度混乱的自然场景中的模态完成问题。Ling 等人提出了一个用于非模态补全的变分自编码器框架，称为 Amodal-VAE[12]，它在训练时不需要任何非模态标签，因为它能够利用广泛可用的对象实例分割来生成用于自监督学习的数据。Nguyen 等人提出 ASBU[13] 方法，对PCNet 进行改进，通过不确定性加权来隐式地进行学习对象形状先验，使用这种不确定性加权的损失函数对训练进行约束，对分割结果预测一张不确定性图，用估计的不确定性对损失函数加权来正则化模型训练，从而在具有高不确定性的区域中产生较低的分割损失。条件生成对抗网络 (cGAN)[14] 已被用于不同的应用，例如未来帧的预测、样式转移、边缘图的着色和合成图像等。SeGAN[15] 将 cGAN 与卷积网络相结合，以同时分割和绘制对象的遮挡区域，但是其问题与修复不同，因为目标是在输入掩码之外绘制区域，而图像修复是在已知区域进行内容的填补。SeGAN 除了完成了被遮挡对象的分割掩码，还为每个对象实例的被遮挡区域生成了外观，并且展示了从合成图像到自然图像的转换。Kar 等人[16] 使用关键点注释将 3D 对象模板与 2D 图像对象对齐，从而生成非模态边界框的真实值。CARAFE[17] 利用 3D 合成数据来训练端到端的非模态补全网络。尽管上述方法在图像非模态补全结果上有很大进步，但是这些方法依然存在以下问题：信息流不连贯，上述方法普遍将非模态形状补全和非模态外观补全分别使用两个不同的模型实现，容易导致信息流不连贯，预测的图像和掩码不匹配，得到的非模态外观补全结果不够自然、真实；训练困难，对于需要同时训练两个模型的方法，需要协调两个模型的训练过程，增加了训练难度和时间成本。



**2.1Amodal shape completion**
**amodal shape completion 方法**：
（第2.1.1节和第2.1.2节讨论）
1. 依赖object detection
2. 没有object detection，直接directly performs instance segmentation

2.1.1 Instance segmentation methods with object detection
主要采用两阶段的方法，即检测后分割。
1. CNN-based methods
2. CompositionalNets variants
3. Weakly supervised learning

2.1.2. Instance segmentation methods without object detection
1. Traditional methods
2. CNN-based methods
3. Weakly supervised learning

2.1.3. Other types of amodal shape representation


