## SVD

### 1. 矩阵分解

1.1导入相关库
```python
from numpy import *
from numpy import linalg as la
```

1.2数据集

```python
def loadExData():
  return[[0, 0, 0, 2, 2],
         [0, 0, 0, 3, 3],
         [0, 0, 0, 1, 1],
         [1, 1, 1, 0, 0],
         [2, 2, 2, 0, 0],
         [5, 5, 5, 0, 0],
         [1, 1, 1, 0, 0]]
```
1.3进行矩阵分解
```python
data = loadExData()
U,Sigma,VT = la.svd(data)
```

```python
>>> U
array([[  0.00000000e+00,   5.34522484e-01,   8.37548868e-01,
          1.01909305e-01,   4.91131461e-02,   6.52171142e-17,
         -2.39321450e-17],
       [  0.00000000e+00,   8.01783726e-01,  -4.90973792e-01,
         -2.70288776e-01,   2.07440522e-01,   3.05311332e-16,
         -6.93889390e-17],
       [  0.00000000e+00,   2.67261242e-01,  -2.02176360e-01,
          6.07047720e-01,  -7.20547857e-01,  -9.84967805e-16,
          2.20308414e-16],
       [ -1.79605302e-01,   1.85980476e-17,   4.72088531e-02,
         -2.71408992e-01,  -2.41903050e-01,   8.95649199e-01,
         -1.76481855e-01],
       [ -3.59210604e-01,   3.71960953e-17,   9.44177062e-02,
         -5.42817984e-01,  -4.83806099e-01,  -4.44717778e-01,
         -3.68183963e-01],
       [ -8.98026510e-01,   1.06817028e-17,  -5.66506237e-02,
          3.25690790e-01,   2.90283659e-01,   3.49368814e-16,
         -9.13994065e-17],
       [ -1.79605302e-01,   1.85980476e-17,   4.72088531e-02,
         -2.71408992e-01,  -2.41903050e-01,  -6.21364261e-03,
          9.12849782e-01]])
```
```python
>>> Sigma
array([  9.64365076e+00,   5.29150262e+00,   7.17576917e-16,
         7.36147444e-17,   1.74873814e-33])
```
矩阵Sigma以行向量的形式返回，因为只有对角线元素非零。前两个值比其他值大了很多，于是可以将最后三个值去掉


Sigma计算能量
```python
>>> SigEnergy = Sigma**2
>>> sum(SigEnergy)
121.0
```
再计算总能量的90%：
```python 
>>> sum(SigEnergy) * 0.9
108.90000000000001
```

计算前两个元素所包含的能量：
```python
>>> sum(SigEnergy[:2])
121.0
```
该值高于总能量的90%。

```python
>>> VT
array([[ -5.77350269e-01,  -5.77350269e-01,  -5.77350269e-01,
          0.00000000e+00,   0.00000000e+00],
       [ -2.16480700e-16,   1.08240350e-16,   1.08240350e-16,
          7.07106781e-01,   7.07106781e-01],
       [ -7.18866271e-01,   2.41370514e-02,   6.94729219e-01,
         -1.83624811e-16,  -2.39135962e-16],
       [ -3.87166569e-01,   8.16139736e-01,  -4.28973168e-01,
         -7.02311690e-17,   4.07911335e-17],
       [  0.00000000e+00,   1.10582250e-16,  -1.10582250e-16,
         -7.07106781e-01,   7.07106781e-01]])
```
用上面三个矩阵相乘得到原始数据集的一个近似.

先构建一个2\*2的矩阵Sig2
```python
Sig2 = mat([[Sigma[0], 0], [0, Sigma[1]]])
```
```python
>>> U[:, :2]*Sig2*VT[:2, :]
matrix([[ -6.12299885e-16,   3.06149942e-16,   3.06149942e-16,
           2.00000000e+00,   2.00000000e+00],
        [ -9.18449827e-16,   4.59224914e-16,   4.59224914e-16,
           3.00000000e+00,   3.00000000e+00],
        [ -3.06149942e-16,   1.53074971e-16,   1.53074971e-16,
           1.00000000e+00,   1.00000000e+00],
        [  1.00000000e+00,   1.00000000e+00,   1.00000000e+00,
           6.95875223e-17,   6.95875223e-17],
        [  2.00000000e+00,   2.00000000e+00,   2.00000000e+00,
           1.39175045e-16,   1.39175045e-16],
        [  5.00000000e+00,   5.00000000e+00,   5.00000000e+00,
           3.99672720e-17,   3.99672720e-17],
        [  1.00000000e+00,   1.00000000e+00,   1.00000000e+00,
           6.95875223e-17,   6.95875223e-17]])
```

### 2.基于协同过滤的推荐引擎

相似度计算(参数都必须为整型)
- 基于欧式距离
```python
def ecludSim(inA,inB):
    return 1.0/(1.0 + la.norm(inA - inB))
```
- 基于皮尔逊系数
```python
def pearsSim(inA,inB):
    if len(inA) < 3 : return 1.0
    return 0.5+0.5*corrcoef(inA, inB, rowvar = 0)[0][1]
```
检查是否存在3个或更多的点。如果不存在，该函数返回1.0，这是因为此时两个向量完全相关。

－ 基于余弦
```python
def cosSim(inA,inB):
    num = float(inA.T*inB)
    denom = la.norm(inA)*la.norm(inB)
    return 0.5+0.5*(num/denom)
```
测试一下

```python
>>> data = mat(data)
>>> ecluSim(data[:, 0], data[:, 4])
0.12973190755680383
```
第一列与第二列数据的欧式距离相似度为0.129732

```python
>>> cosSim(data[:, 0], data[:, 4])
0.5
```
余弦相似度为0.5

```python
>>> pearsSim(data[:, 0], data[:, 4])
0.20596538173840329
```
基于皮尔逊系数相似度为0.205965

## 3.基于物品相似度的推荐引擎
计算在给定相似度计算方法的条件下，用户对物品的估计评分值。参数：数据矩阵、用户编号、物品编号和相似度计算方法。
```python
def standEst(dataMat, user, simMeas, item):
    n = shape(dataMat)[1]
    simTotal = 0.0; ratSimTotal = 0.0
    for j in range(n):
        userRating = dataMat[user,j]
        if userRating == 0: continue
        # 找到任意两个用户都对j和item评分的集合
        overLap = nonzero(logical_and(dataMat[:, item].A>0, dataMat[:, j].A>0))[0]
        if len(overLap) == 0: 
            similarity = 0
        else:
            similarity = simMeas(dataMat[overLap, item], dataMat[overLap, j])
            print 'the %d and %d similarity is: %f' % (item, j, similarity)
        simTotal += similarity
        ratSimTotal += similarity * userRating  # 相似度
    if simTotal == 0:
        return 0
    else:
        return ratSimTotal/simTotal
```
用所有用户对某两个物品的评分来计算相似度。预测评分＝similarity_i\*score_i/(sum(similarity_i))

```python
def recommand(dataMat, user, N=3, simMeas=cosSim, estMethod=standEst):
    unratedItems = nonzero(dataMat[user, :].A == 0)[1]
    if len(unratedItems) == 0:
        return 'you rated everything'
    itemScores = []
    for item in unratedItems:
        estimatedScore = estMethod(dataMat, user, simMeas, item)
        itemScores.append((item, estimatedScore))
    return sorted(itemScores, key=lamda jj: jj[1], reverse=True)[:N]
```

测试一下，数据集
```python
def loadExData2():
    return[[0, 0, 0, 0, 0, 4, 0, 0, 0, 0, 5],
           [0, 0, 0, 3, 0, 4, 0, 0, 0, 0, 3],
           [0, 0, 0, 0, 4, 0, 0, 1, 0, 4, 0],
           [3, 3, 4, 0, 0, 0, 0, 2, 2, 0, 0],
           [5, 4, 5, 0, 0, 0, 0, 5, 5, 0, 0],
           [0, 0, 0, 0, 5, 0, 1, 0, 0, 5, 0],
           [4, 3, 4, 0, 0, 0, 0, 5, 5, 0, 1],
           [0, 0, 0, 4, 0, 4, 0, 0, 0, 0, 4],
           [0, 0, 0, 2, 0, 2, 5, 0, 0, 1, 2],
           [0, 0, 0, 0, 5, 0, 0, 0, 0, 4, 0],
           [1, 0, 0, 0, 0, 0, 0, 1, 2, 0, 0]]
           
data = mat(loadExData2())
```

给第3个用户的推荐
```python
>>> recommend(data, 2)
the 0 and 4 similarity is: 0.000000
the 0 and 7 similarity is: 0.990916
the 0 and 9 similarity is: 0.000000
the 1 and 4 similarity is: 0.000000
the 1 and 7 similarity is: 0.978429
the 1 and 9 similarity is: 0.000000
the 2 and 4 similarity is: 0.000000
the 2 and 7 similarity is: 0.977652
the 2 and 9 similarity is: 0.000000
the 3 and 4 similarity is: 0.000000
the 3 and 7 similarity is: 0.000000
the 3 and 9 similarity is: 1.000000
the 5 and 4 similarity is: 0.000000
the 5 and 7 similarity is: 0.000000
the 5 and 9 similarity is: 1.000000
the 6 and 4 similarity is: 1.000000
the 6 and 7 similarity is: 0.000000
the 6 and 9 similarity is: 0.692308
the 8 and 4 similarity is: 0.000000
the 8 and 7 similarity is: 0.995750
the 8 and 9 similarity is: 0.000000
the 10 and 4 similarity is: 0.000000
the 10 and 7 similarity is: 1.000000
the 10 and 9 similarity is: 1.000000
[(3, 4.0), (5, 4.0), (6, 4.0)]
```
```python
>>> recommend(data, 2, simMeas=ecludSim)
the 0 and 4 similarity is: 0.000000
the 0 and 7 similarity is: 0.414214
the 0 and 9 similarity is: 0.000000
the 1 and 4 similarity is: 0.000000
the 1 and 7 similarity is: 0.289898
the 1 and 9 similarity is: 0.000000
the 2 and 4 similarity is: 0.000000
the 2 and 7 similarity is: 0.309017
the 2 and 9 similarity is: 0.000000
the 3 and 4 similarity is: 0.000000
the 3 and 7 similarity is: 0.000000
the 3 and 9 similarity is: 0.500000
the 5 and 4 similarity is: 0.000000
the 5 and 7 similarity is: 0.000000
the 5 and 9 similarity is: 0.500000
the 6 and 4 similarity is: 0.200000
the 6 and 7 similarity is: 0.000000
the 6 and 9 similarity is: 0.150221
the 8 and 4 similarity is: 0.000000
the 8 and 7 similarity is: 0.500000
the 8 and 9 similarity is: 0.000000
the 10 and 4 similarity is: 0.000000
the 10 and 7 similarity is: 0.200000
the 10 and 9 similarity is: 0.500000
[(3, 4.0), (5, 4.0), (6, 4.0)]
```

```python
>>> recommend(data, 2, simMeas=pearsSim)
the 0 and 4 similarity is: 0.000000
the 0 and 7 similarity is: 0.961547
the 0 and 9 similarity is: 0.000000
the 1 and 4 similarity is: 0.000000
the 1 and 7 similarity is: 0.750000
the 1 and 9 similarity is: 0.000000
the 2 and 4 similarity is: 0.000000
the 2 and 7 similarity is: 0.750000
the 2 and 9 similarity is: 0.000000
the 3 and 4 similarity is: 0.000000
the 3 and 7 similarity is: 0.000000
the 3 and 9 similarity is: 1.000000
the 5 and 4 similarity is: 0.000000
the 5 and 7 similarity is: 0.000000
the 5 and 9 similarity is: 1.000000
the 6 and 4 similarity is: 1.000000
the 6 and 7 similarity is: 0.000000
the 6 and 9 similarity is: 1.000000
the 8 and 4 similarity is: 0.000000
the 8 and 7 similarity is: 0.990098
the 8 and 9 similarity is: 0.000000
the 10 and 4 similarity is: 0.000000
the 10 and 7 similarity is: 1.000000
the 10 and 9 similarity is: 1.000000
[(3, 4.0), (5, 4.0), (6, 4.0)]
```
基于不同相似度计算方法，推荐的菜品及相应得分竟然是一样的

实际的数据集会是一个稀疏矩阵。利用SVD降维。

先对data矩阵进行SVD分析

```python
>>> U, Sigma, VT = la.svd(data)
>>> Sigma
array([ 15.77075346,  11.40670395,  11.03044558,   4.84639758,
         3.09292055,   2.58097379,   1.00413543,   0.72817072,
         0.43800353,   0.22082113,   0.07367823])
>>> SigEnergy = Sigma**2
>>> sum(SigEnergy)
541.9999999999992
>>> sum(SigEnergy)*0.9
487.79999999999927
>>> sum(SigEnergy[:3])
500.50028912757909
```
前3个元素所包含的能量高于总能量的90%

基于SVD的评分估计
```python
def svdEst(dataMat, user, simMeas, item):
    n = shape(dataMat)[1]
    simTotal = 0.0; ratSimTotal = 0.0
    U, Sigma, VT = la.svd(dataMat)
    Sig3 = mat(eye(3)*Sigma[:3])
    xformedItems = dataMat.T * U[:, :4] * Sig3.I
    for j in range(n):
        userRating = dataMat[user, j]
        if userRating == 0 or j == item:
            continue
        similarity = simMeas(xformedItems[item,:].T, xformedItems[j,:].T)
        simTotal += similarity
        ratSimTotal += similarity * userRating
    if simTotal == 0:
        return 0
    else:
        return ratSimTotal/simTotal
```
进行推荐
```python
>>> recommend(data, 2, estMethod=svdEst)
the 0 and 4 similarity is: 0.487100
the 0 and 7 similarity is: 0.996341
the 0 and 9 similarity is: 0.490280
the 1 and 4 similarity is: 0.485583
the 1 and 7 similarity is: 0.995886
the 1 and 9 similarity is: 0.490272
the 2 and 4 similarity is: 0.485739
the 2 and 7 similarity is: 0.995963
the 2 and 9 similarity is: 0.490180
the 3 and 4 similarity is: 0.450495
the 3 and 7 similarity is: 0.482175
the 3 and 9 similarity is: 0.522379
the 5 and 4 similarity is: 0.506795
the 5 and 7 similarity is: 0.494716
the 5 and 9 similarity is: 0.496130
the 6 and 4 similarity is: 0.434401
the 6 and 7 similarity is: 0.479543
the 6 and 9 similarity is: 0.583833
the 8 and 4 similarity is: 0.490037
the 8 and 7 similarity is: 0.997067
the 8 and 9 similarity is: 0.490078
the 10 and 4 similarity is: 0.512896
the 10 and 7 similarity is: 0.524970
the 10 and 9 similarity is: 0.493617
[(6, 3.0394902391812892), (5, 3.0090087051508894), (3, 3.0058579857590901)]
```

```python
>>> recommend(data, 2, simMeas=pearsSim,estMethod=svdEst)
the 0 and 4 similarity is: 0.491930
the 0 and 7 similarity is: 0.995589
the 0 and 9 similarity is: 0.525351
the 1 and 4 similarity is: 0.490161
the 1 and 7 similarity is: 0.995215
the 1 and 9 similarity is: 0.524614
the 2 and 4 similarity is: 0.490351
the 2 and 7 similarity is: 0.995258
the 2 and 9 similarity is: 0.524687
the 3 and 4 similarity is: 0.450126
the 3 and 7 similarity is: 0.319482
the 3 and 9 similarity is: 0.566918
the 5 and 4 similarity is: 0.528504
the 5 and 7 similarity is: 0.118324
the 5 and 9 similarity is: 0.590049
the 6 and 4 similarity is: 0.423669
the 6 and 7 similarity is: 0.588901
the 6 and 9 similarity is: 0.562042
the 8 and 4 similarity is: 0.495434
the 8 and 7 similarity is: 0.996232
the 8 and 9 similarity is: 0.526835
the 10 and 4 similarity is: 0.544647
the 10 and 7 similarity is: 0.113370
the 10 and 9 similarity is: 0.602380
[(10, 3.730155458905462), (5, 3.7130085211532116), (3, 3.2828825144760154)]
```
