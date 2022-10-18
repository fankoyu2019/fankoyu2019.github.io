---
title: 论文笔记|csorfs-finder
tags:
 - 论文笔记
 - orf
date: 2022-10-7 16:33
categories:
 - 论文笔记
---

## csORF-finder: an effective ensemble learning framework for accurate  identification of multi-species coding short open reading frames

### 摘要

短开放阅读框（sORFs）是指长度不超过303  nt的小核片段，可能编码小肽。迄今为止，在信使核糖核酸（RNAs；mRNAs）的非翻译区和长非编码RNAs（lncRNAs）中都发现了可翻译的sORFs，在无数的生物过程中发挥着重要作用。由于并非所有的sORFs都被翻译或基本可被翻译，因此开发一个高度精确的计算工具来描述sORFs的编码潜力是非常重要的，从而促进发现新的功能性肽类。有鉴于此，我们通过整合EfficientCapsNet和LightGBM设计了一系列集合模型，统称为csORF-finder，以区分智人、小鼠和黑腹果蝇中的编码sORFs（csORFs）与非编码sORFs。为了提高csORF-finder的性能，我们引入了一种新的特征编码方案，**即三核苷酸偏离预期平均值（TDE）**，并计算了所有类型的基于框内序列的特征，如i-framed-3mer、i-framed-CKSNAP和i-framed-TDE。基准测试结果表明，与原始的3-mer、CKSNAP和TDE特征相比，这些特征可以显著提高性能。我们的性能比较表明，csORF-finder在多物种和非ATG启动的独立测试数据集上取得了比最先进的csORF预测方法更好的性能。此外，我们应用csORF-finder筛选lncRNA数据集，以确定潜在的csORFs。所得数据作为一个重要的计算资源库，用于进一步的实验验证。我们希望csORFfinder可以作为一个强大的平台，用于高通量识别csORFs和对这些csORFs编码的肽进行功能鉴定。

<!--more-->

### Introduction

在这项工作中，我们设计并实现了一个新的计算框架csORF-finder，它建立在一系列集成模型的基础上，通过整合胶囊网络和自我注意路径(Efficient-CapsNet)[32]和光梯度增强机器(LightGBM)[33]，分别识别智人、小鼠和黑腹果蝇中的csORF。总共构建了6个模型，用于预测这三个物种的编码区和非编码区(CDS和非CDS)的csORF。重要的是，我们在Carr等人的启发下提出了一种新的特征编码方案--三核苷酸偏离预期平均值(TDE)。[34]其用于表征sORF序列中三核苷酸的相关性。为了确保提取的特征具有生物学意义，我们根据三个平移框架计算了给定sORF序列的I-Framed-3mer、I-Framed-CKSNAP和I-Framed-TDE，与原始的3-mer、CKSNAP和TDE特征相比，显著提高了模型的性能。使用Efficient-CapsNet和LightGBM的最佳特征和集成，csORF-finder在10倍交叉验证测试中取得了出色的性能，并在各种独立测试数据集上超过了几种最先进的csORF预测方法。更重要的是，随着越来越多的证据表明csORF也存在于lncRNAs中，我们进一步应用csORF-finder来筛选智人、肌肉支原体和黑腹水母的所有lncRNA数据集，并生成这些物种潜在的csORFs的计算纲要。这些预测的csORFs为研究界提供了有用的资源，可以作为进一步的实验验证和csORFs生物学功能的新假说产生的高度可信的候选者。

### 材料与方法

#### 框架设计

![image-20221009180101141](论文笔记_csorfs-finder/image-20221009180101141.png)

CsORF-finder旨在精确识别csORF。CsORF-finder的开发包括六个主要步骤(图1)，包括数据采集和预处理、序列编码、特征选择、模型构建、参数优化和性能评估。首先使用从SmProt数据库[7，8]收集的实验验证的csORF和来自EnSembl[35]、NONCODE[36]和NCBI  RefSeq数据库[37]的过滤后的非csORF来组装三个不同物种(即智人、肌肉和黑腹夜蛾)的高度自信的基准数据集。然后，基于sORF及其编码的多肽提取了数千个信息特征。接下来，基于LightGBM特征重要性排名的结果，使用增量特征选择(IFS)方法确定每个物种的最佳特征组合[38]。预测模型是通过集成LightGBM算法和Efficient-CapsNet构建的，其超参数使用贝叶斯优化算法进行调整和优化[39]。最后，通过10倍交叉验证测试和独立测试对csORFfinder的分类性能进行了综合评价。

#### 数据收集和预处理

##### Collection of csORFs and non-csORFs

我们最初从SmProt[7]中提取了39 293条、10 242条和21 594条经过实验验证的小肽（≤100个氨基酸），分别用于H. sapiens、M.  musculus和D. melanogaster，证据标准严格，包括 "质谱法"（MS）证据、"文献挖掘 "证据和 "已知数据库  "证据。对SmProt的这些证据标准的详细描述见补充T able S1。使用CD-HIT程序[40]去除序列冗余后，在H. sapiens、M.  musculus和D. melanogaster中分别保留了31 082、8811和20  860条小肽。对于每个物种，我们使用SmProt数据库中提供的Ensembl [35], NONCODE [36] 和NCBI RefSeq [37] I D  s将这些小肽映射到它们的转录本上。所有以 "ATG "开头的sORFs（≤303个核苷酸）都是从映射的转录本序列中提取的。这些sORFs的冗余被CD-HIT  -EST程序[40]去除，序列同一性阈值为0.8。由于sORFs可以在mRNAs和ncRNAs的CDS和非CDS中找到（图2A），因此根据sORFs的位置指导阳性（即csORF）和阴性（即非csORF）数据集的构建。

通过序列比对，与SmProt中确认的小肽对应的sORFs被认为是csORFs。结果，H. sapiens、M. musculus和D.  melanogaster共剩下41  324个csORFs。根据这些csORFs的位置，我们将这些csORFs分成两个子集，包括mRNA中的csORFs和ncRNA中的csORFs。然后，我们进一步将mRNAs中的csORFs分为CDS-sORFs（即那些在mRNAs的CDS中的csORFs）和非CDS-sORFs（即那些在mRNAs的非CDS中的csORFs）。因此，对于每个物种，我们构建了三个数据集，包括CDS-sORFs、非CDS-sORFs和ncRNA-sORFs。对这三个物种分别重复了这种正样本选择程序（图2B）。为了研究不同物种和位置的csORFs的密码子使用偏好，我们使用ggseqlogo  [41]对H. sapiens、M. musculus和D.  melanogaster的CDS-sORFs和非CDS-sORFs数据集分别进行了启动密码子之后（图3A-C）和终止密码子之前（补充图S1）的三个密码子序列分析。

非csORF是指不编码实验验证的多肽的不可翻译的sORF，即那些不能与从SmProt中提取的小肽比对的sORF。由于在一些sORF中显示的Ribo-Seq阅读的3核苷酸周期性可以有力地支持翻译[42]，在这里我们使用‘核糖体剖析’(RP)证据来进一步筛选非csORF。因此，在SmProt中可以翻译成具有RP证据的小肽的那些非csORF被进一步去除，从而提高了所选择的真正的非csORF的置信度。然后，得到的非csORF根据它们的位置被分成三个数据集。同样，这一过程也分别在这三个物种中重复。补充图S2中显示了上述数据集构建过程的图形说明。我们还绘制了小提琴图来说明长度分布，并比较了每个物种的csORF和非csORF数据集的差异(图3D-F)。

##### 构建训练和测试数据集

对于每个物种，我们采用CDS-sORFs数据集来构建CDS定位的csORF预测器（即CDS-sORFs），非CDS-sORFs数据集用于非CDS定位的csORF预测器（即非CDS-sORFs），ncRNA-sORFs数据集用于独立测试和与其他预测器的性能比较。在使用CDS-sORFs和非CDS-sORFs数据集时，我们将其分成80%作为训练数据集用于模型的构建和参数优化，20%作为独立测试数据集用于与已发表的方法进行性能比较。

此外，我们注意到SmProt数据库刚刚发布了2.0版本(http://bigdata.ibp.ac.cn/SmProt/)，该版本对Ribo-seq分析管道进行了优化，加上人工确认，确保了小肽的可靠性[8]。因此，我们从SmProt  v2.0中收集了H.sapiens和M.musculus最新的带有Ribo-seq证据的csORFs（命名为RibosORFs数据集），以考察其通用性能。还下载了其他生物的csORFs，包括Rattus  norvegicus, Danio rerio, Caenorhabditis elegans, Saccharomyces  cerevisiae和Arabidopsis  thaliana，以评估跨物种预测性能。非csORFs是从Ensembl数据库中每个物种的所有转录本中收集的，这些转录本被注释为传统的非编码生物类型（如miRNA-微RNA，rRNA-核糖体RNA，tRNA-转移RNA，snRNA-小核RNA和snoRNA-小核RNA等）。考虑到非ATG起始密码子也是无处不在且具有生物学意义的，我们还提取了H.  sapiens和M.  musculus的非ATG起始密码子的csORFs作为额外的独立测试样本。多物种独立测试数据集的构建见补充方法S1。每个物种的数据集的详细分类见T able  1。

#### 序列编码

为了提取能够有效区分csORFs和非csORFs的信息特征，我们挖掘了sORFs的核苷酸和翻译肽序列特征。基于核苷酸的特征包括i-framed-3mer、i-framed-CKSNAP、i-framed-TDE、hexamer score和ORF长度；而基于肽的特征是氨基酸组成（AAC）、氨基酸对组成（AAPC）和组成、转换和分布（CTD）。除i-framed-TDE外，所有类型特征的描述都在补充方法S2中实现。

##### i-framed-TDE

在这项研究中，我们提出了一个新的基于核苷酸的特征，名为TDE，它可以被表述为：
$$
TDE = \frac{T_{c}(xyz)-T_m(xyz)}{T_v(xyz)}
$$
上述公式中的三个参数，包括三核苷酸组成（Tc）、理论平均值（Tm）和理论差异（Tv），分别计算如下。
$$
T_c(xyz) = N_{xyz}/N_{total}, x,y,z \in \{{A,C,G,T\}}\\
T_m(xyz) = \frac{\sum T_c(xyz)}{N_{POS}+N_{NEG}}\\
T_v(xyz) = \sqrt{\frac{T_m(xyz)(1-T_m(xyz))}{N_{total}}}
$$
其中，Nxyz是三核苷酸xyz的出现次数，Ntotal代表查询序列中三核苷酸的总数，NPOS和NNEG分别是训练数据集中的阳性和阴性样本的数量。

而对于i-framed-TDE，Nxyz是不重叠的三核苷酸xyz的数量，Ntotal是第i个阅读框中不重叠的3-mers的总数（图2C）。停止密码子（即TAA、TAG、TGA）被排除在第一阅读框之外。因此，i-framed-TDE特征的总维度为189。在此，我们比较了这三个帧内特征（即i-framed-3mer、i-framed-CKSNAP和i-framed-TDE）的组合与原始3-mer、CKSNAP和TDE特征的预测性能。对由两种特征组合构建的每个模型进行了10倍交叉验证测试。

#### 特征选择

基于上述的序列编码方案，我们为每个sORF构建了一个1110维的特征向量。对于D. melanogaster  CDS-sORFs数据集来说，特征的数量远远超过了序列的数量，这将导致模型的过度拟合。此外，计算这样一个高维的特征向量并训练模型是非常耗费资源和时间的。因此，我们采用了两步的特征选择策略，以去除不相关的特征，并确定更重要的特征以优化预测性能。在第一步中，分别使用五种流行的特征选择方法来计算每个特征的得分，包括Chi-squared（Chi2）[43]、Pearson相关系数（PCC）[44]、最大信息系数（MIC）[45]、F-core[46]和LightGBM[38]的特征重要性测量。然后，将步长为100的IFS方法[47]分别应用于五个排序的特征列表，以生成最佳特征子集。接下来，生成的特征子集被用作构建机器学习模型的输入数据。每种特征排序方法确定的最优特征子集的性能见T  able 2。使用前n个特征（[50, 1110], step =  100）按五种特征排名方法排序的F1得分（F1）在补充图S3中显示了不同特征数量下的性能。在10倍交叉验证测试的基础上，使用平衡精度和召回率的F1来评估最佳特征组合的性能[48,  49]。如果两个特征组合之间出现平局（即达到相同的F1值），那么将选择准确率较高的组合。

#### 模型构建与参数优化

总共建立了6个模型，分别用于预测智人、肌肉和黑腹鱼CDS(即使用CDS-sORFs数据集)和非CDS(即使用非CDS-sORFs数据集)中的csORFs。我们分别训练了Efficient-CapsNet[32]和LightGBM[33]模型，并通过对这两个模型的概率得分进行平均来计算最终的预测得分(图1D)。在模型构建过程中，从训练数据集中随机抽取10%的样本作为验证数据进行参数优化。LightGBM要求用户调整多个参数(补充T表S2)，以确保模型的精度和稳健性。考虑到网格搜索方法很耗时，我们使用了贝叶斯优化算法[39]来微调关键参数。模型的详细结构和超参数可以在补充方法S3和S4中找到。

#### 绩效评估

为了评估csORF-Finder的性能，我们使用了7项测量，包括灵敏度(Sn)、特异度(Sp)、准确度(ACC)、精确度、F1、马修相关系数(MCC)[50]和ROC曲线下面积(AUC)。计算公式可参考补充方法S5。

### 结果和讨论

#### 不同物种间csORFs的序列分析

如图3A-C所示，CDS-sORFs数据集的序列徽标表示表明，起始密码子之后最受欢迎的三个密码子分别为：智人的‘gag’、‘gag’和‘gag’，肌肉的‘gag’、‘Gac’和‘gag’，黑腹夜蛾的‘Gac’、‘Aac’和‘GTC’。而对于非CDSsORFs数据集，最受欢迎的三个密码子分别是：智人的GAG、GAG和GTG，肌肉的GCG、GCG和GAG，黑腹鱼的CAA、ATA和AAT。至于终止密码子之前的首选三个密码子，这种差异更加显著(补充图S1)。

由于假定的ORF在非编码序列中很少见[30]，ORF长度是用于区分mRNAs和ncRNAs的关键特征[51]。如图3D-F所示，csORF明显长于非csORF。唯一的例外是来自黑腹水母非编码区的sORF的长度分布。与智人和肌肉鼠相比，该地区黑腹鼠的sORFs一般较短。然而，MS严重依赖于现有文库的覆盖，因此主要关注长蛋白。SmProt数据库中MS证据支持的小蛋白由ncRNAs(补充T能力S1)编码；而用于长度分布分析的csORF序列基于来自蛋白质编码RNA的CDS-sORFs和非CDS-sORFs数据。因此，MS支持的csORF序列不包括在长度分布分析中。可以认为，ORF长度是预测csORFs的基本和关键特征之一。

#### 帧内特征可以提高预测性能

在这项研究中，我们提出了基于核特征的帧内特征（即i-framed-3mer、iframed-CKSNAP和i-framed-TDE）。这三个帧内特征的组合与原始3-mer、CKSNAP和TDE特征的性能比较结果见图4和补充T  able  S3。很明显，结合帧内特征可以明显提高预测性能。对于H.sapiens、M.musculus和D.melanogaster的CDS-sORFs数据集，使用帧内特征训练的模型优于使用原始特征训练的模型，性能分别提高了12.2%、13.9%和13.0%的Acc，23.8%、27.7%和26.0%的MCC，10.2%、12.9%和13.8%的F1。一个可能的原因是，帧内特征能更好地反映蛋白质编码信息，体现sORF序列的生物学意义。另一方面，在每个物种的非CDS-sORFs数据集上，帧内特征组合的改进不太明显，特别是对于D.  melanogaster，使用原始特征组合训练的模型只取得了略高的Sp（约1.8%）和精度值（约0.4%）。这表明在非编码区区分csORF和非csORF序列比在mRNA的编码区更具挑战性。研究结果还表明，其他可能有用的特征，如ORF长度和六聚体得分，应进一步纳入模型以提高预测性能。

#### LightGBM和IFS的结合是最有效的特征选择策略

在本节中，我们考察了各特征排序方法（Chi2、F-s c o r e、P C C、M I C或LightGBM）与IFS策略在H. sapiens、M.  musculus和D. melanogaster数据集上的优化预测性能。从T able  2和补充图S3可以看出，无论数据集类型或物种如何，LightGBM以最少的特征数量获得了最佳的整体性能。相比之下，其他的特征排名方法偶尔会取得稍高的Sp、Sn和精度值。例如，使用H.sapiens数据集，PCC在CDS-sORFs数据上取得了更高的Sp和精度值，在非CDS-sORFs数据上取得了更好的Sn。然而，值得注意的是，与LightGBM相比，PCC利用了1000多个特征来获得稍好的Sn、Sp和精度值，而LightGBM只用了150-250个特征来获得有竞争力的Acc、MCC、F1和AUC值。在M.  musculus和D.  melanogaster的特征选择结果中也可以看到类似的情况。总的来说，这种特征选择方法的比较分析表明，结合LightGBM和IFS提供了一种有效的策略，用于排序和选择信息量最大的特征以优化预测性能。

我们进一步评估了LightGBM和IFS特征选择策略在H.sapiens、M.musculus和D.melanogaster的CDS-sORFs和非CDS-sORFs数据集上分别获得的最佳特征子集的各个特征类型的重要性和性能贡献（图5A）。这些单独的特征类型包括六聚体得分、ORF长度i-framed-3mer、i-framed-CKSNAP、i-framed-TDE、AAC、AAPC和CTD。图5B清楚地表明，每个单独的特征类型都取得了合理的预测性能，而这些特征的组合（即最佳特征集）取得了最佳的预测性能。由此可以得出结论，基于帧内序列的特征在提高预测性能的最佳特征集中起着关键作用。我们还评估了csORFs和非csORFs中hexamer分数的分布（图5C）。在每个模型中，csORFs和非csORFs的六聚体得分值有明显的差异，这表明使用六聚体得分来提高预测性能的潜力很大。

#### LightGBM和Efficient-CapsNet的集合模型代表了csORF预测的最佳解决方案

我们分别使用H. sapiens、M. musculus和D.  melanogaster的CDS和非CDS数据集构建了总共6个预测模型来识别csORFs。对于每个模型，基于LightGBM和IFS确定的最佳特征集，LightGBM和Efficient-CapNet的超参数是根据模型在验证数据集上的表现确定的。最终选定的每个模型的最佳超参数和相应的AUC值在补充T能S4中提供。

基于10倍交叉验证测试，LightGBM和Efficient-CapsNet的集成模型在使用最优特征集时取得了出色的预测性能(表3)。有趣的是，我们发现从非CDS-sORFs数据集预测csORFs比从CDSsORFs数据集预测csORFs更具挑战性。例如，使用150个优化特征从智人CDS-sORFs数据集中预测csORFs的ACC率为84.91%，F1值为0.850；而在智人非CDS-sORFs数据集上使用250个特征时，ACC率为82.68%，F1值为0.827。然而，这两个数据集之间的平均AUC值的差异很小(补充图S4)。此外，模型的性能是特定于物种的。集成模型对肌肉分枝杆菌的预测效果最好，而在预测mRNAs非编码区的黑腹分枝杆菌csORFs时，性能差异显著。相似的数据特征可能是解释在非CDS-sORFs数据集上预测黑腹果蝇csORFs的性能相对较差的可能原因。

然后，我们通过10倍交叉验证，使用六个训练数据集比较了我们的集成模型(即LightGBM+Efficient-CapsNet)与传统的机器学习和深度学习算法，包括支持向量机(SVM)、随机森林(RF)、LR、LightGBM、深度神经网络(DNN)、卷积神经网络(CNN)和Efficient-CapsNet的预测性能(图6和补充T表S5)。结果表明，我们的集成策略在预测CDS和非CDS区域的csORFs方面取得了显著的性能，并优于所有其他基准算法。具体地说，比较中使用的CNN模型由三个卷积层、三个最大汇集层、一个紧凑层、两个致密层和一个丢失层组成。DNN模型包含四个致密层和三个批次归一化层，其中线性单元整流和S型激活。CNN和DNN在所有物种上的性能都显著低于Efficient-CapsNet，这表明胶囊网络学习特征之间的空间位置关系的能力可以有效地提高模型的性能。令人惊讶的是，尽管基于支持向量机的分类器已经被应用于各种编码RNA预测工具，如CPC[24]、CNCI[25]和CPPred[28]，但在本工作中，支持向量机在预测csORF方面表现不佳。

#### CsORF-finder在csORF预测方面优于最先进的方法

表4总结了以前开发的预测csORF的计算方法的关键方面，并与csORF-finder进行了比较。在这项工作中，我们只使用独立的测试数据集与其他现有的公开可用的csORF识别方法对我们提出的csORF-finder方法进行了基准测试。这些方法包括CPPred[28]、DeepCPP[29]、Cpat[30]、RNAsamba[31]和MiPepid[21]。

如图7A和B以及补充TableS6所示，csORF-finder在所有三个物种的CDS和非CDS数据集上的表现都优于比较的方法。例如，在智人CDS-sORFs数据集上，与所有其他方法相比，csORF-finder在ACC中至少提高了13.7%，在精度上提高了12.4%，在F1中提高了14.1%。对于每个物种，Cpat和RNAsamba根据性能指标排名第二好的预测因子，表现优于MiPepid。尽管比csORF-finder有更好的特异值，DeepCPP和CPPred分别对智人和黑腹果蝇的非CDS-sORFs数据集产生了显著较低的敏感值，这表明这两种方法在识别UTR序列中的编码信号方面的准确性较低。

然后，我们评估了这些方法在预测位于LncRNAs中的csORF。最近的测序技术已经发现，许多LncRNA包含可以编码小功能肽的sORF[11]。因此，除了mRNAs中的sORF数据集(即CDS-sORF和非CDS-sORF)外，还提取了lncRNAs的sORF区域作为额外的独立测试数据集(即ncRNA-sORF)，以进一步评估csORF-finder的性能(图7C和补充T表S7)。对于LncRNAs中csORFs的识别，我们应用了基于每个物种的非CDS-sORFs数据集的模型。结果表明，csORF-finder在三个ncRNA-sORFs数据集上的性能低于在CDS/非CDS数据集上的性能，但在ACC、MCC、精度和F1方面仍优于其他方法。因此，为了识别和注释可翻译的lncRNAs，我们应用csORF-finder分别在智人、肌肉杆菌和黑腹水鼠的lncRNAs中生成了可能的csORFs的计算概要。这三个物种的lncRNAs摘自集合数据库[35](Release  99；ftp://ftp.ensembl.org/pub/release-99/Fasta/)。删除了短序列(<=200个核苷酸)和含有‘ACGT’以外不常见核酸的序列。所得到的数据集分别包含50109、15615和2257个lncRNAs，分别用于智人、肌肉和黑腹鱼。包括预测分数在内的预测结果的详细描述在补充测试表S8中提供。

近年来，Ribo-Seq技术揭示了“以前认为的”非编码区的翻译活动。建立在非CDS-sORFs数据集上的csORF-finder模型使用Ribo-sORFs数据集和带有非ATG起始密码子(参见材料和方法)的csORFs数据集与其他五种方法进行了比较。预测结果如图7D，补充表S9和S10所示。详细的业绩比较见补充结果S1。

#### 在预测其他模式生物中的csORF时，csORF-finder具有通用性和健壮性

为了评估构建在智人、肌肉和黑腹夜蛾sORF数据集上的csORF-finder对其他物种的泛化能力，我们测试了它在多物种sORF数据集上的性能，这些数据集包括褐家鼠、黑腹夜蛾、秀丽隐翅虫、酿酒隐翅虫和黑腹夜蛾。如图7E所示，在六个模型中，智人和肌肉非CDS-sORFs模型在其他物种上取得了令人满意的预测性能，无论物种之间的进化距离如何，这表明csORFfinder可能是一个通用的计算平台来预测多种类型的生物，如脊椎动物、真菌和植物。基于跨物种预测结果，我们对褐家鼠、酿酒酵母和刺参的测试数据采用了智人非CDS-sORFs模型，对白纹伊蚊和线虫的测试数据采用了肌肉非CDSsORFs模型。进一步进行了与五个最先进的csORF预测器的比较实验(图7F和补充T表S11)。在灵敏度、ACC和F1方面，csORFfinder在预测褐家鼠、白纹伊蚊、酿酒酵母和拟南星天牛csORFs方面优于其他方法，而在预测酿酒酵母csORFs方面，所有方法的预测效果都较差。

### 结论

在本文中，我们开发了csORF-finder，一个由一系列物种特有的集成模型组成的计算平台，通过集成Efficient-CapsNet和LightGBM分别识别智人、肌肉和黑腹鱼CDS和非CDS区域的csORF。独立测试表明，csORF-finder的性能优于其他最先进的csORF预测方法。考虑到在进行详尽的实验之前，sORF不能被确信是不可翻译的，在未来，我们将应用正向无标记学习算法来解决这个问题，其中只需要正的(即实验验证的csORF)和未标记的样本(即可以是可翻译的或不可翻译的sORF)[52，53]。

评估包括帧内特征在内的特征在预测编码长ORF方面的有效性，以及是否可以构建一个通用模型来预测长编码ORF和短编码ORF，也是很有意义的。CsORF-finder的模型架构也可以应用于或定制到其他物种的基因组中，例如病毒，以识别新的csORF，从而为人类传染病及其治疗提供有用的见解。综合考虑，我们预计csORF-finder将作为一个工具性的生物信息学工具包，用于执行csORF的高通量预测分析并指导实验验证。

## 要点

- 我们总结了预测csORF的最新方法和数据来源

- CsORF-finder是一个新的基于集成学习的平台，它集成了Efficient-CapsNet和LightGBM模型，显著提高了多物种csORF的预测精度

- 为了提高分类性能，提出了一种新的特征表示方法TDE。

- 利用csORFfinder获得了智人、肌肉和黑腹水母的cncRNAs的csORFs概要，为进一步的实验验证和功能鉴定提供了有价值的资源。