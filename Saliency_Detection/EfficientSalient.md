# 论文笔记 - Efficient Salient Region Detection with Soft Image Abstraction
> 该论文发表在ICCV 2013

## 亮点
该论文是受SF方法的启发，对SF进行的一个改进。

其大致改进有两个地方：图像abstraction方法的改进（由K-means硬聚类转变成GMM软聚类）;对于global-constract 的提取上采用GC方法进行优化。

## 算法
> 对于物体显著性算法的演变，从pixel-based 到现在的region-based(image abstraction)；从local-contrast 到现在的 global-contract。

- 提取图像abstraction

1. 首先使用GMM对量化的颜色特征(从颜色值0-255转化为0-12)进行聚类。此后，便对每一块区域进行计算颜色直方图。
2. 由于简单的进行GMM聚类，没有考虑到空间等信息，会导致相近空间区域会变成两类。所以，作者便计算每个类别的相似度（依靠GMM得出的概率）。

```math
C(c_i, c_j) = \frac{\sum_{I_x}min(P(c_i|I_x), P(c_j|I_x)} {min(\sum_{I_x}P(c_i|I_x), \sum_{I_x}P(c_j|I_x)}
```
其中`$P(c_j|I_x)$` 为像素`$I_x$`属于`$c_j$`的概率。这里对P进行·硬化·，即只有top 2 的类别为1 其余为0。

此后，使用message-passing based clustering 的方法对GMM的结果进行聚类。便可以合并一些相近区域的类别。

- 计算constract/uniqueness

有两种不同的方法：
1. Global uniqueness (Gu)

该计算在GMM聚类结果上


```math
U(c_i) = \sum_{c_j \ne c_i} exp(\frac{D(c_i, c_j)}{-\sigma^2}) * w_{c_i} * ||\mu_{c_i}-\mu_{c_j}||
```
其中D表示区域的空间距离。`$\mu$`表示中间区域的颜色直方图值

2. Color spatial distribution (CSD)

该计算是在message-passing clustering 的结果上。



