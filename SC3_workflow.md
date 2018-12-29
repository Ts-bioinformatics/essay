# SC3: consensus  clustering of single-cell  RNA-seq data 

## WorkFlow：
![]()
### 第一步：输入文件
   输入文件为表达量矩阵，但是需要事先进行预处理（如标准化），以使数据可以在样本内和（或）样本间进行比较
### 第二步：基因过滤
   基因过滤用于有两个标准，二选其一（见下，By default, X is set at 6），其原因是The motivation for the gene filter is that ubiquitous 
and rare genes are most often not informative for clustering，即去除掉对分类结果没什么影响的基因，以达到减少运行时间的目的。实验结果显示基因过滤操作对分类结果没有显著影响。

      （1）expressed (expression value > 2) in less than X% of cells (rare genes/transcripts) 
      （2）expressed (expression value > 0) in at least (100 – X)% of cells (ubiquitous genes/transcripts)

### 第三步：计算细胞间的距离

  在计算距离之前，对表达量数据进行了对数转换（M' = log2(M + 1)，M为原输入表达量矩阵)，这一步是为后续的细胞分类及分型做数据准备，共计算了3种距离——Euclidean、Pearson和Spearman距离。
  
### 第四步：数据转换降维  

  这一步使用了两种降维方法——PCA和graph Laplacian，分别对3种距离进行降维（共计由3*2 = 6组数据）。最终选用的特征值（eigenvector）的数目定为4% ~ 7% of cells。降维同样是为了较少运算量，减少运行时间。
  
### 第五步：k-means聚类

  这一步分别对6组数据进行k-menas clustering with Hartigan and Wong algorithm，每一组数据分别进行7% of cells - 4% of cells = 3% of cells次聚类（将每个特征值数量都运行一遍，如果次数超过15次，那么从中随机挑选15个特征值运行，后续的一致矩阵构建用到了这些数据）。
  
### 第六步：构建Consensus matrix

  其实到第五步就可以结束了，因为它已经产生了分类结果信息，但研究结果显示Consensus clustering结果对于结果的稳定性和准确性都有提升。对于第五步得到的6*3% of cells个聚类结果，针对每个细胞，统计每个细胞分类一致的结果数比例；如果一个细胞在6*3% of cells个聚类结果中分类都一样，那么其比列为1，相反，如果一个细胞在6*3% of cells个聚类结果中分类结果都不一样，那么其比列为0，这就是一致性（consensus）含义。为了构成矩阵，行和列都是细胞，矩阵关于对角线对称，这就是一致矩阵（consensus matrix）。
  
### 第七步：一致聚类（consensus cluster）
  即对一致矩阵进行complete-linkage hierarchical clustering，k值可以自己指定或者软件估计，最终得到分类结果。
   


    
        

