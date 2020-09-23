# Review on papers



by Yuanke zhong

***



### Spatial dataset

* Paper：Visualization and analysis of gene expression in tissue sections by spatial transcriptomics

* Data set：

  * Breast cancer: Layer 1-4 ；

  * Mouse olfactory bulb: MOB replicate 1-12

  count matrix: 

  https://www.spatialresearch.org/resources-published-datasets/doi-10-1126science-aaf2403/ 

  location info: 

  https://github.com/SpatialTranscriptomicsResearch/st_pipeline/tree/master/ids 
  ​	问题：目前只找到了同一个样本，不同replicate的数据，没有找到同一组织，不同slice的数据，是否可以对不同replicate的数据进行registration，是否有意义？

  ​	RNA-seq中，重复的设计是实验设计不可缺少的一部分： 实验中的不稳定性和样本之间差异引起的误差。 能不能达到我们所期望的统计功效。 在分析基因差异表达时，也就是最终得到多少差异显著的基因。有助于FDR的改善。

 



***

### Spatial DE

* 论文:

* meaning :找出spatial variance genes，和传统HVG互补，不重复

* 方法：spatial gaussian process 回归

  1、假设高斯过程的核函数，推导出边缘似然函数，用极大似然估计求出未知参数集（θk, σ2s, δ, µ）

  2、用不同的covariance function 对高斯过程进行评估，求得不同的参数

* 实验

 

***

### Spatial mapping

* 论文:

意义\****：

spatial数据可以知道转录的位置信息，但是不知道是哪一个细胞进行的转录，而scRNA-seq数据的每一个转录都有对应的单个细胞。所以结合两种技术的数据来描述细胞类型群体的空间形貌的想法是令人信服的。

***\*Assumption:\****

ST数据和scRNA-seq数据都服从 NB 分布

***\*数据要求：\****

选择具有相似 cell type content的scRNA-seq和ST数据

***\*论文使用数据\****：

1.human development heart

2.mouse brain

***\*方法分为两个步骤\****：

1.使用scRNA-seq数据，根据MLE（极大似然估计）确定NB分布的参数\

2.假设ST数据与scRNA-seq数据共享参数，使用第一步生成的参数在前面乘上一个系数，这个系数代表每个spot上每种细胞类型的参数的贡献度（取值范围0-1），则每个spot的细胞类型为贡献度最大细胞类型。



***



### Seurat

A unified strategy for reference assembly and transfer learning for transcriptomic, epigenomic, proteomic and spatially-resolved single cell data.

* Significance：

  1. single cell ATAC-seq (scATAC-seq) can uniquely reveal enhancer regions and regulatory logic, but currently may not achieve the same power for unsupervised cell type discovery as transcriptomics.

   2.Through the identification of cell pairwise correspondences between single cells across datasets, termed "anchors", we can transform datasets into a shared space, even in the presence of extensive technical and/or biological differences. This enables the construction of harmonized atlases at the tissue or organismal scale。

  3.Integrate these datasets into a harmonized atlas that can be used to better understand cellular identity and function.

* Pipline：

  1）数据预处理和feature selection

  2）降维并找出anchor

  3）对anchor进行评价筛选

  4）Data correction/transform

* methods 

  1、Preprocessing

  首先对数据进行Log normalization，选择2000个high variance gene

  2、降维并选择anchor

  CCA：使用SVD求出两个数据集相应的优化向量u，v，然后对每个细胞求得一个CCV（Canonical Correlation Vectors）。

  然后对于某个数据集的每个细胞，在另一个数据集中找出K-nearest neighbors，文章称为MNN。则每个细胞和他的一个邻居就是一个anchor。

  3、对anchor进行评估

  ​	a) Anchor scoring：用原始高维的数据对anchor中存在的数据再求其MNN，这里的高维首先选取与CCV相关性最大的前200个基因，如果anchor中的另一个细胞被识别为了MNN，则保留这个anchor，否则就去除。用SNN计算一个score，并用score计算一个权重，score越低，anchor在后面计算的权重越小。

  ​	b) Anchor weighting：由两因素决定，上面算出的score和anchor的distance。 

  4、integration

 

***



 

 