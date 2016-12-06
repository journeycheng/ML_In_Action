# ML_In_Action

Sklearn Matplotlib seaborn Numpy Pandas Numpy Scipy

```python
>>> from numpy import *
>>> random.rand(4,4)  # array
>>> randMat = mat(random.rand(4,4))
>>> randMat.I # inverse of a matrix
>>> eye(4)
```

normalizing numeric values

必须考虑两个问题：一、使用机器学习算法的目的，想要算法完成何种任务；二、需要分析或收集的数据是什么
如果想要预测目标变量的值，则可以选择监督学习算法，否则可以选择无监督学习算法。确定选择监督学习算法之后，需要进一步确定目标变量类型，如果目标变量是离散值，则可以选择分类器算法；如果目标变量是连续型的数值，则需要选择回归算法。


如果不想预测目标变量的值，则可以选择无监督学习算法。进一步分析是否需要将数据划分为离散的组。如果这是唯一的需求，则使用聚类算法；如果还需要顾及数据与每个分组的相似程度，则需要使用密度估计算法。

应该充分了解数据，对实际数据了解得越充分，越容易创建复合实际需求的应用程序。主要应该了解数据的以下特性：特征值是离散型变量还是连续型变量，特征值中是否存在缺失的值，何种原因造成缺失值，数据中是否存在异常值，某个特征发生的频率如何，等等。充分了解上面提到的这些数据特性可以缩短选择机器学习算法的时间。

## k-近邻算法
- 优点：精度高、对异常值不敏感、无数据输入假定
- 缺点：计算复杂度高、空间复杂度高
- 适用数据范围：数值型和标称型
- 工作原理：存在一个样本数据集合，也称作数据样本集，并且样本集中每个数据都存在标签，即我们知道样本集中每一数据与所属分类的对应关系。输入没有标签的新数据，将新数据的每个特征与样本集中数据对应的特征进行比较，然后算法提取样本集中特征最相似（最近邻）的分类标签。一般来说，我们只选择样本集中前k个最相似的数据，这就是k-近邻算法中k的出处，通常k是不大于20的整数。最后，选择k个最相似数据中国年出现次数最多的分类，作为新数据的分类。
- 错误率：分类器给出错误结果的次数除以测试数据的总数


点与点的距离计算公式。在处理不同范围的特征值时，通常采用的方法是将数值归一化，如将取值范围处理为0到1或者－1到1之间。

0～1之间：
```python
newValue = (oldValue-min)/(max-min)
```
每个测试向量做距离计算，每个距离计算包括多维度的浮点运算。

k决策树：k近邻算法的优化版

## 决策树
k近邻算法可以完成很多分类任务，但是它最大的缺点就是无法给出数据的内在含义，决策树的黄祖耀优势就在于数据形式非常容易理解。

- 优点：计算复杂度不高，输出结果易于理解，对中间值的缺失不敏感，可以处理不相关
- 缺点：可能会产生过度匹配的问题
- 使用数据类型：数值型（必须离散化）和标称型

在构造决策树时，需要解决的第一个问题就是，当前数据集上哪个特征在划分数据分类时起决定性作用。为了找到决定性的特征，划分出最好的结果，必须评估每个特征。


遍历当前特征中的所有唯一属性值，对每个特征划分一次数据集，然后计算数据集的新熵值，并对所有唯一特征值的到的熵求和。

信息增益是熵的减少或者数据无序度的减少

决策树分类器就像带有终止块的流程图，终止块表示分类结果。构建决策树时，通常采用递归的方法将数据集转换为决策树。

其他的决策树的构造算法，最流行的是C4.5和CART


## 朴素贝叶斯
- 优点：在数据较少的情况下，可以处理多类别问题
- 缺点：对于输入数据的准备方式较为敏感
- 使用数据类型: 标称型数据。
- 选择具有最高概率的决策


文本特征相互
