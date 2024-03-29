---
title: 论文笔记|RSG：一种简单而有效的不平衡数据集学习模型
tags:
 - 论文笔记
date: 2022-4-13 18:45:01
categories:
 - 论文笔记
---

## RSG: A Simple but Effective Module for Learning Imbalanced Datasets

### 摘要

不平衡数据集在实际中广泛存在，对于训练具有较好泛化能力的非频繁类的深层神经模型是一个巨大的挑战。在这项工作中，我们提出了一个新的稀有类样本生成器(RSG)来解决这个问题。RSG的目标是在训练过程中为稀有类生成一些新的样本，特别是它具有以下优点：(1)使用方便，通用性强，因为它可以很容易地集成到任何一种卷积神经网络中，并且在结合不同的损失函数时效果很好，(2)它只在训练阶段使用，因此在测试阶段不会给深层神经网络带来额外的负担。在广泛的实验评估中，我们验证了RSG的有效性。此外，通过利用RSG，我们在不平衡的CIFAR上获得了有竞争力的结果，并在Places-LT、ImageNet-LT和iNaturalist  2018上获得了最新的结果。源代码可在https://github.com/Jianf-Wang/RSG.上找到

<!--more-->

### Introduction

非平衡数据集可以根据数据分布分为两类：**长尾非平衡分布[6]和阶跃非平衡分布[2]，这两种分布都将是本文的重点。**

在训练过程中为稀有类生成新的样本是一个很好的解决方案[8，36，38]，它被认为是一种数据增强方法。然而，这些方法都有不同的缺点，限制了它们的性能。首先，一些框架[8，38]没有以端到端的方式进行训练，因此梯度不能从CNN的顶部反向传播到底部。但众所周知，深度模特通常可以受益于端到端的培训。其次，一些方法[8，38]利用同一频繁类样本之间的变化信息，如不同的姿势或光照，来生成新的稀有类样本。然而，这些方法没有引入任何机制来确保从频繁类获得的变异信息是与类无关的。因此，如果将变异信息(仍然包含类相关信息)直接与真实的稀有类样本相结合来生成新的稀有类样本，用于训练分类器和重塑决策边界，则会由于不同类相关信息的混叠而影响性能。最后，Wanget等人[36]使用噪声向量对上述变异信息进行编码。但是使用这样的噪声向量来生成可能会产生不稳定或低质量的样本，因为噪声向量太随机而不能反映真实图像之间的真实变化。

为了克服上述缺陷，本文提出了一种简单而高效的全参数生成器，称为稀有类样本生成器(RSG)，该生成器可以端到端地训练任何主干。RSG直接利用同一频繁类的真实样本中反映不同姿态或光照的变化信息来生成新的样本，而不是使用随机向量对这些信息进行编码，因此RSG可以生成更合理和稳定的样本。此外，RSG还引入了一个新的模块，旨在进一步过滤掉变异信息中可能存在的与频繁类相关的信息，解决了上述混叠问题.

图1显示了如何将其集成到针对不平衡数据集的简单CNN中。RSG只需要任意特定层样本的特征图，在训练过程中产生一些新的样本来影响稀有类，以调整它们的决策边界，扩大它们的特征空间。在测试阶段，删除了RSG，因此不会给网络带来额外的计算负担。请注意，我们在图1中只显示了一个简单的CNN，但RSG可以用于任何网络体系结构，如ResNet[11]、DenseNet[15]、ResNeXt[35]和Inception[27]。

> 产生标签为稀有类的，新的特征向量？

![image-20220406185819442](论文笔记_RSG：一种简单而有效的不平衡数据集学习模型/image-20220406185819442.png)

### Related Work

目前用于处理不平衡数据集的现有解决方案主要可以分为基于重采样和重加权、新的损失函数、元学习、利用未标记数据和样本生成的方法。

重采样技术包括对少数类[25，2，3，1，19]进行过采样和对多数类[2，18，10]进行欠采样，目的是平衡数据分布。重加权方法[13，14，34，6，21]也试图通过给频繁类样本和稀有类样本分配不同的权重来平衡数据分布。一些方法[39，4]通过直接添加约束来设计新的损失函数来影响频繁类和稀有类的决策边界。为了解决数据不平衡问题，还提出了一些基于元学习的方法[24，17，16]。最近，Yang和Xu[37]分析了不平衡标签的价值，并利用未标记的数据通过半监督和自我监督策略来促进类平衡学习。

与其他方法相比，以前的样本生成方法与这项工作更相关。一种hallucinator[36]被设计用来为稀有类别生成新样本。它使用稀有类的真实实例和噪声向量来为稀有类产生新的hallucinator实例。提出了一种用于生成新样本的∆编码器框架[8]。它首先被训练来从频繁类中重建输入图像的预先计算的特征向量。然后，通过组合真实的稀有类样本生成新的样本，并进一步用新生成的样本训练分类器。最近提出了一种特征转移学习(FTL)框架[38]，它由自动编码器、特征过滤器和全连接(FC)层组成。自动编码器最初是在一个大规模的数据集上进行预训练，通过几个epoch来收敛学习潜在表征。然后，通过生成一些新的稀有类样本，利用主成分分析(PCA)将类内方差从频繁类转移到稀有类。提出了一种两阶段交替训练策略，对编码层、特征滤波层和FC层进行联合优化。

### Rare-Class Sample Generator (RSG)

稀有类样本生成器(RSG)由中心估计模块、对比模块和向量变换模块组成(见图2)。为了优化RSG的参数，采用了两种损失函数，即样本对比中心估计(CESC)损失和最大向量损失(MV)损失。

![image-20220409171945066](论文笔记_RSG：一种简单而有效的不平衡数据集学习模型/image-20220409171945066.png)



RSG假设一个类别的样本服从单峰分布或多峰分布[17，38]，因此在每个类别中可以有一个或一组中心来适应分布。在本文中，我们定义了**特征位移的概念，它是指由于同一物体在输入图像中具有不同的条件(如角度、姿态或光线条件)而导致的样本在类中到其对应中心的位移。**因此，在理想情况下，特征位移不应包含与类相关的信息。

给定一个由频繁类和稀有类实例组成的小批量样本，RSG将它们的特征映射作为输入，并将它们转发到这些模块。中心估计模块的目的是估计每一类中的一组中心，将这些中心作为“anchors”来获得每个样本的特征位移。对比模块用于确保在样本生成过程中特征位移不包含任何与频繁类相关的信息。向量变换模块根据估计的中心计算每个频繁类样本的特征位移，并利用它为稀有类生成新的样本。直观地说，为稀有类生成一些具有这样的特征位移的新样本可能会缓解由于稀有类通常缺乏输入变量而导致的数据集不平衡的问题。

#### 中心估计模块

公式如下：

![image-20220409170740609](论文笔记_RSG：一种简单而有效的不平衡数据集学习模型/image-20220409170740609.png)

其中X^l^∈R^D×W×H^是输入样本的特征映射，我们假设通道维度、宽度和高度分别为D、W和H。L是样本的类别标签，ave(·) 表示跨宽度和高度的全局平均池化，A^l^和B^l^是对输入执行线性变换的该模块的参数，f是输出概率分布(γ^l^)的Softmax函数，用于将样本分配到其对应类别中最接近的中心。

由于类内数据分布未知，中心估计模块被设计为估计一组中心，而不是每个类只估计一个中心。如果类内数据分布是多模式分布，则使用一组中心比使用单个中心要好。相反，如果是单峰分布，这些中心可以非常接近或重叠，这类似于使用单个中心。

#### 对比模块

公式如下：

![image-20220409170919178](论文笔记_RSG：一种简单而有效的不平衡数据集学习模型/image-20220409170919178.png)

其中，x^1^∈R^D×W×H^和x^2^∈R^D×W×H^是给定小批次中任意两个输入样本的特征映射，cat(·)表示沿通道维度执行的级联操作。h(·)是通过叠加两个3×3卷积层和一个RELU激活层来实现的，该卷积层具有256个通道。A^∗^和b^∗^是线性层的参数，从而产生概率分布γ^∗^，以显示两个样本是否来自同一类别。

#### 向量变换模块

负责将真实频繁类样本和真实稀有类样本的特征位移相结合，生成新的稀有类样本。如图1所示，不平衡的数据集导致决策边界中的偏差，导致稀有类的特征空间小于频繁类的特征空间。因此，我们提出使用向量变换模块来为稀有类生成新的样本，以扩大特征空间并“推开”决策边界。

为了生成新的样本，首先需要从频繁类中获取特征位移，这是通过使用中心估计模块估计的频繁类样本及其对应的中心来实现的：

![image-20220409172401850](论文笔记_RSG：一种简单而有效的不平衡数据集学习模型/image-20220409172401850.png)

其中x^l^~freq~∈R^D×W×H^表示频繁类L中的样本，我们用C^l^~i~∈R^D^来表示类L中的第i个中心，其维度为D，K是离x^l^~freq~最近的中心的指标，即K=argmax f(A^l^ave(x^l^~freq~)+b^l^)。up(·)  表示通过沿宽度和高度重复C^l^~i~的值实现的上采样操作，形成与x^l^~freq~ 大小相同的中心的特征图。在从x^l^~freq~中减去相应的中心特征映射后，从x^l^~freq~中去除了大部分与类相关的信息；因此，我们使用**x~fd-freq~来表示频繁类样本的特征位移。**

> x~fd-freq~ 应该是一个向量

然后，第二步是通过使用x~fd-freq~和真实的稀有类样本来生成新的稀有类样本。直观上，x~fd-freq~可以加到稀有类的中心，但我们直接将x~fd-freq~加到真实的稀有类样本中，有两个原因：首先，一些x~fd-freq~的长度可能小于稀有类中特征空间的原始方差。如果我们把x~fd-freq~加到中心，新的样本可能对决策边界没有影响。其次，由于稀有类的样本量有限，大部分稀有类样本可以直接决定决策边界，在稀有类样本中加入x~fd-freq~对决策边界的影响更为直接。

> 直接加在稀有类的样本上，而不是加在中心

所以，新的稀有类样本的生成过程是：

![image-20220409173253418](论文笔记_RSG：一种简单而有效的不平衡数据集学习模型/image-20220409173253418.png)

其中x^l′^~rare~∈R^D×W×H^表示稀有类l′中的样本，x^l′^~new~是该类中新产生的样本，T是一个线性变换，定义为T（z）=conv（z），其中conv表示一个单一的卷积层，包含一组卷积滤波器，核大小为3，跨度为1，填充大小为1，其数量与输入特征图的通道数相同。

#### **带有样本对比损失的中心估计**(LCESC)

目的是更新每个类别的中心，优化对比模块和中心估计模块。因此，它由两个经典的损失项组成，可以写成：

![image-20220409173436986](论文笔记_RSG：一种简单而有效的不平衡数据集学习模型/image-20220409173436986.png)



其中，d、j和k表示沿通道、宽度和高度的特征图的索引。γ^l^~i~是由公式（2）得到的样本属于第i个中心的概率，K是每个类的中心数，s是批次大小。考虑批量大小为 s 的小批量，在对比模块的训练期间，通过从小批量中随机挑选样本来形成 s/2 样本对。我们**用 y ∈ {0, 1} 表示每个输入对中的样本是否来自同一类的真值。**<·>s和<·>s/2表示L~CESC~的第一项和第二项分别在s个实例和s/2个配对中平均计算。

#### 最大化矢量损失（LMV）

优化了向量变换模块的参数（即T），确保新生成的样本能够扩大稀有类的特征空间，其基本思想是最大化新生成样本相对于其中心的特征位移（即图3中新的整体特征位移）。在此，我们将样本的特征位移视为从中心到样本的一个向量（见图3）。为了生成一个新的稀有类样本，可以直接将x~fd-freq~加入到稀有类样本中（图3，左），但x~fd-freq~的方向通常是不确定的，而且由于三角形不等式的存在，新的整体特征位移通常不一定具有最大的长度。因此，我们设计了MV loss，使变换后的向量与稀有类样本的特征位移同向共线，并使变换后的向量的长度不变（图3，右），以最大限度地影响决策边界。例如，如果使用直接加法，由于总长度有限，新生成的样本可能不会影响决策边界。但是利用向量转换模块和MV损失，可以确保新生成的样本广泛分布在稀有类的特征空间中，因为相对于中心的位移较大，它可以提高训练期间每批新生成的样本能够出现在决策边界周围的概率。

![image-20220409180644197](论文笔记_RSG：一种简单而有效的不平衡数据集学习模型/image-20220409180644197.png)

此外，对于一个给定的常量级样本，虽然在通过公式（3）计算样本的特征位移时，常量级相关信息已经被基本去除，但我们仍然使用对比模块来确保特征位移不包含常量级相关信息，以进一步缓解新的稀有类样本生成时可能出现的类相关信息混杂问题。W.l.o.g.，γ^∗^是对比模块的两个输入样本不属于同一类别的概率，MV损失为:

![image-20220409180819261](论文笔记_RSG：一种简单而有效的不平衡数据集学习模型/image-20220409180819261.png)

其中j和k表示特征映射沿宽度和高度的索引，|·|取绝对值，x~fd-rare~表示通过公式(3)从样本与其稀有类中最近的中心获得的特征位移。对比模块的两个输入样本分别是T(x~fd-freq~)和x^l^~freq~。LMV的第一项本质上是最小化T(x~fd-freq~)和x~fd-rare~的余弦角，以使它们在同一方向上共线，第二项是保持T(x~fd-freq~)的长度与x~fd-freq~不变，第三项使T(x~fd-freq~)和x^l^~freq~不属于同一类别，确保T(x~fd-freq~)不会有任何频繁类相关信息。给定一小批样本，<·>s~new~表示对新生成的样本按平均值计算LMV，其中s~new~是新生成的样本数。

注意，最小化LMV可能会鼓励生成一些具有非常大的总体特征位移的新样本，这可能会损害频繁类的性能。因此，向量变换模块还接收来自分类损失函数L~cls~的梯度，在L~cls~和LMV的第二项之间达到权衡，从而为稀有类生成更合理的新样本。

RSG的训练过程和总损失函数在算法1中总结，分别给出如下：

![image-20220413154431929](论文笔记_RSG：一种简单而有效的不平衡数据集学习模型/image-20220413154431929.png)



![image-20220409182332202](论文笔记_RSG：一种简单而有效的不平衡数据集学习模型/image-20220409182332202.png)

其中，L~cls~表示分类损失，例如带有交叉熵损失的 softmax, focal loss [23]、AM-Softmax[30，31]和LDAM[4]，并且λ1和λ2表示系数。本文将epoch阈值T~th~设置为学习率衰减到 0.001 的 epoch 的索引。

RSG的工作流程如下(见图2)：在epoch阈值T~th~到达之前，给定一小批样本，RSG根据手动设置的恒定频繁类比率α=n~freq~/n~cls~将它们分成两部分，其中n~freq~和/n~cls~分别表示频繁类的数目和类的总数。例如，对于10个类别的训练集，α=0.3时，样本数量最多的三个类别为频繁类别，其他类别为稀有类别。(请注意，为简单起见，在图2中仅绘制了频繁类和稀有类。)。然后，将数据转发到中心估计模块，以更新每一类的中心，并优化中心估计模块的参数。此外，这些数据也被转发到对比模块以优化其参数。在epoch阈值T~th~到达之后，RSG 开始生成新样本，**并且对比模块的参数不再更新**。频繁类中每个样本的特征位移由向量变换模块计算，然后用 T 进行变换，并以手动设置的参数传递强度 β 随机添加到稀有类的数据中，从而产生新的样本。对比模块将梯度传播到向量变换模块中的 T 以优化 T 并过滤掉与频繁类相关的信息。一般来说，在给定的  mini-batch 中，频繁类中的样本数不小于稀有类中的样本数。我们将**转移强度 β  定义为参与计算特征位移和为稀有类生成新样本的频繁类中的样本数。**具体来说，新生成的样本数为 s~new~ = max{⌊β×s~freq~/s~rare~⌋,  1}×s~rare~，其中 s~freq~ 和 s~rare~ 分别是 mini-batch 中频繁类和稀有类的样本数，⌊·⌋是下限函数。最后，将新生成样本的特征图与原始输入特征图沿批量维度连接起来，并转发到后续层以计算损失并优化整个框架。

### 实验评估

#### 数据集

实验评估的重点是非平衡CIFAR、iNaturist  2018、Places-LT和ImageNet-LT数据集。非平衡CIFAR基于原始CIFAR数据集，该数据集是通过减少每个类的训练样本而构建的，并且验证集不变。不平衡比ρ被定义为最频繁类和最不频繁类的样本大小之间的比率，即ρ=N~max~/N~min~。我们在长尾不平衡[6]和步长不平衡[2]设置上进行了实验。我们在实验中使用的不平衡因子(ρ)是50和100。INaturist物种分类数据集[29]是一个大规模的不平衡数据集，包含437,513个训练图像，在其2018年版本中归类为8142个物种。官方培训和验证集分别具有长尾分布和均衡分布。Places-LT有365个类别，每类最大4980个图像，最小5个图像；ImageNetLT有1000个类别，每类最大1280个图像，每类最小5个图像。至于对这两个数据集的评估，将类别进一步划分为三个部分：多镜头(超过100个样本)、中等镜头(在20到100个之间)和少镜头(小于20个)，以便更好地检查在训练过程中看到的不同样本数的类别之间的性能差异。我们遵循前人工作[4，19]中这些数据集的实验设置进行评估。

#### 实施细节

关于这四个数据集的培训细节摘要如下：

**不平衡CIFAR**：我们遵循基本的数据增强方法[11]进行训练：填充4个像素，从图像或其水平翻转中随机裁剪一个32×32的块。该框架以128的批次大小进行了200个epoch的训练。学习速率最初设置为0.1，然后在第160个epoch衰减0.01，在第180个epoch再次衰减。采用动量为0.9的随机梯度下降法对网络进行优化。

**INaturist  2018**：我们遵循标准做法，从图像或其水平翻转中进行随机大小裁剪[27]至224×224的数据增强。该网络从零开始训练了90个纪元，批次大小为256。学习速率最初设置为0.1，然后分别在第50个epoch、第70个epoch和第85个epoch衰减0.1。此外，为了进行公平的比较，我们跟踪了Kang等人。[19]并针对2×调度(180个epoch)训练了模型。在我们的2×时序实验中，学习速度分别在第100、140和170个时期衰减了0.1。在验证过程中，图像被中心裁剪到224×224，而不需要进一步放大。

**Places-LT**：我们遵循以前的工作[24]来执行数据增强和微调ResNet-152，该ResNet-152是在完整的ImageNet-2012数据集上预先训练的。该网络以256的批次大小训练了30个时期。初始学习速率设为0.01，每10个周期衰减0.1个周期，30个周期后停止训练。

**ImageNet-LT**：我们遵循以前的工作[19]使用ResNeXt-50-32x4d，它经过100个epoch的256次训练。将初始学习速率设置为0.1，并分别在第60个epoch、第80个epoch和第95个epoch衰减0.1。

#### 消融研究

我们对ρ=50的不平衡CIFAR进行了消融研究。报告了从三个独立运行中获得的平均错误率。通过使用LDAM-DRW[4]在ResNet32[11]上进行实验，我们全面搜索了RSG的超参数，并探索了哪种特征级别最适合RSG生成新的样本，其中“DRW”是指曹等人提出的一种延迟重加权训练策略。[4]。在我们探索的基础上，在后续的实验中，我们将长尾和阶梯不平衡分布的中心数设置为15，频类比分别设置为0.2和0.5，长尾和阶梯不平衡分布的传递强度分别设置为1.0和0.01，λ1和λ2分别设置为0.1和0.01。搜索过程可在补充材料中找到。请注意，RSG最初是在倒数第二个下采样层之前使用的。

首先，我们将网络架构固定为ResNet-32[11]，并针对不同的L~cls~对RSG进行了测试。根据表1，当结合不同的损失函数时，配备RSG的Depth模型的性能一致好于没有RSG的Depth模型。当RSG与具有交叉熵损失(表示为ERM，即经验风险最小化)的标准Softmax相结合时，RSG显著提高了性能。这是合理的，因为标准的Softmax没有任何针对不平衡数据集的机制。对于focal loss、AM-Softmax和LDAM，尽管它们都设计得很好，可以处理不平衡的数据集，但RSG仍然可以进一步提高性能。

![image-20220410150200645](论文笔记_RSG：一种简单而有效的不平衡数据集学习模型/image-20220410150200645.png)

其次，我们将L~cls~设置为带有DRW[4]的LDAM(即LDAM-DRW)，并结合RSG在不平衡的CIFAR上评估(主要是五种)不同的网络体系结构，即ResNet-32、ResNet-56、ResNet-110、DenseNet-40和ResNeXt-29(8×64D)。请注意，所使用的网络是根据CIFAR在其原始论文[11，35，15]中的实验建立的。如表2所示，当RSG集成到网络中时，所有模型都会得到一致的改进。

![image-20220410150453233](论文笔记_RSG：一种简单而有效的不平衡数据集学习模型/image-20220410150453233.png)

第三，我们对MV损耗和矢量变换模块进行了全面的消融研究，并在表3的基础上得到了以下结论：(1)MV损耗的每个子项都是重要的和有用的，因为一旦我们去掉其中的任何一个子项，就可以观察到错误率的增加。(2)在稀有类的中心加入特征位移，可以提高分类的错误率。这一事实验证了我们在第三节中所提到的，即在真实的稀有类样本中添加特征位移比将其添加到稀有类的中心是更好的选择。(3)使用具有MV损失的矢量变换模比直接对稀有类中的样本添加特征位移效果更好，从而验证了其有效性。

![image-20220410150915263](论文笔记_RSG：一种简单而有效的不平衡数据集学习模型/image-20220410150915263.png)

此外，将RSG与以前的样本生成方法[8，36，38]进行了比较。如表4所示，RSG以不同的幅度超过了以前的方法，表明RSG可以解决前一代方法的缺陷，提高性能。

![image-20220410151130079](论文笔记_RSG：一种简单而有效的不平衡数据集学习模型/image-20220410151130079.png)

最后，我们在ResNet-32的不同池层之前利用RSG来探索哪种级别的特征最适合生成新样本。如表5所示，RSG在倒数第二个下采样层之前使用时效果最好。因此，在剩余的实验中，在倒数第二个下采样层之前仍然使用RSG。

![image-20220410151158161](论文笔记_RSG：一种简单而有效的不平衡数据集学习模型/image-20220410151158161.png)

#### 与最先进的技术进行比较

对于以下每个实验，我们报告平均错误率或平均精度，它们来自三个独立的运行。表6显示了具有ρ∈{50,100}的不平衡CIFAR的结果。我们首先比较了我们的LDAM-DRW-RSG和LDAM-DRW，因为这种比较直接显示了RSG带来的改进。在将LDAM-DRW与RSG相结合后，我们对长尾分布和阶跃不平衡分布都有了显著的改善，这表明了RSG处理不平衡数据集的能力。因此，在RSG的帮助下，LDAM-DRW-RSG在非平衡CIFAR上取得了比以往方法更好的结果。

![image-20220410151406242](论文笔记_RSG：一种简单而有效的不平衡数据集学习模型/image-20220410151406242.png)

表  7 显示了在 iNaturalist 2018 上使用 ResNet-50 [11] 作为主干的不同方法的 top-1 错误率，我们遵循 Kang 等人。  [19] 在两种训练设置中进行实验，即 1× schedular 和 2× schedular。在 1× schedular 实验中，我们分别将  LDAM-DRW-RSG 和 LDAM-DRS-RSG 与之前的 LDAM-DRW 和 LDAM-DRS 进行比较。这里，“DRS”表示 Cao  等人提出的延迟类平衡重采样策略。 [4]。请注意，我们无法使用 LDAM-DRW 重现原始论文 (32.0%) [4] 中报告的 iNaturalist  2018 的结果。因此，我们根据其公开可用的代码报告了 LDAM-DRW 和 LDAM-DRS [4] 的复制结果。表 7  中的结果表明，我们可以通过利用所提出的生成器获得更好的结果，这直接证明了 RSG 的有效性。此外，对于 2x schedular 设置，LDAM-DRS-RSG  的 top-1 错误率进一步降低。因此，可以看出，RSG 帮助模型在两种训练设置中实现了新的最先进的结果，这表明 RSG  能够有效地处理不平衡的数据集。

![image-20220410151413025](论文笔记_RSG：一种简单而有效的不平衡数据集学习模型/image-20220410151413025.png)

表8显示了Places-LT的TOP-1精度。结果表明，将RSG与LDAM-DRS相结合可以进一步提高系统的性能，表明RSG是一种有效的方法。此外，与目前流行的两种方法，即τ归一化方法[19]和BBN方法[1]相比，RSG能够在提高中镜头和少镜头类别的性能的同时，减少多镜头类别的精度损失，从而获得更高的整体精度和新的最先进的结果。

![image-20220410151905759](论文笔记_RSG：一种简单而有效的不平衡数据集学习模型/image-20220410151905759.png)

表9显示了ImageNet-LT的TOP-1精度。与LDAM-DRW相比，LDAM-DRW-RSG能够达到更高的准确率，验证了RSG能够缓解数据集不平衡带来的问题。RSG对模型进行了改进，大大提高了模型对中、少镜头类的通用性。此外，通过配备RSG，我们还可以在ImageNet-LT上获得新的最先进的结果。

![image-20220410152005369](论文笔记_RSG：一种简单而有效的不平衡数据集学习模型/image-20220410152005369.png)

由于RSG的所有超参数在超参数搜索过程后都是固定的，我们可以得出结论，超参数和RSG对新的数据集(即Places-LT、ImageNet-LT和iNaturist  2018)具有相当强的健壮性。如果在新的数据集上进一步调整超参数，可能会获得更好的结果。

### Summary and Outlook

我们引入了一个稀有类样本生成器(RSG)，这是一个通用的构建块，以缓解对不平衡数据集的训练问题。RSG简单而有效，因为它是一个与体系结构无关和与丢失无关的插件模块，并且在推理阶段不会给主干网络带来任何额外的负担。在广泛的实验中，我们验证了RSG的有效性，在四个公共基准上取得了良好的结果。由于RSG的灵活性和与大多数以前方法的正交性，未来的研究可以通过设计更优雅的方法来生成更高质量的稀有类样本来直接改进RSG模块。

