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
- 适用数据类型: 标称型数据。
- 选择具有最高概率的决策


文本特征相互独立



p(c_i|w) = p(w|c_i)p(c_i)/p(w).

如果将w展开为一个个独立特征，那么就可以将p(w|c_i)写作p(w0,w1,...wn|c_i)。假设所有的特征相互独立，还可以写成p(w0|c_i)p(w1|c_i)...p(wn|c_i)。

函数的伪代码：
```
计算每个类别中的文档数目
对每片训练文档：
  对每个类别：
    如果词条出现文档中，增加该词条的计数
    增加所有词条的计数
  对每个类别：
    对每个词条：
    将该词条的数目除以总词条数目得到条件概率
  返回每个类别的条件概率
```
- 根据现实情况修改分类器
利用贝叶斯分类器对文档进行分类时，要计算多个概率的乘积来获得文档属于某个类别的概率，即计算p(w0|1)p(w1|1)...。如果其中一个概率值为0，那么最后的乘积也为0，那么最后的乘积也为0.为降低这种影响，可以将所有词的出现数初始化为1，并将分母初始化为2.


另一个问题是下溢出，这是由于太多很小的数相乘造成的。由于大部分因子都非常小，所以程序会下溢出或者得不到正确的答案。一种解决方法是对乘积取自然对数。在代数中有ln(a\*b)=ln(a)+ln(b)。


遍历词汇表中的每个词并统计它在文本中出现的次数，然后根据次数从高到低对字典排序，最后返回排序最高的100个单词。词汇表中的一小部分单词却占据了所有文本用词的一大部分。


移除高频词效果会更好。产生这种现象的原因是语言中大部分都是冗余和结构辅助性内容。

另一个常用的方法是不仅移除高频词，同时从某个预定此表中移除结构上的辅助词。该词表称为停用词表。

## 逻辑回归

- 优点：计算代价不高，抑郁理解和实现
- 缺点：容易欠拟合，分类精度可能不高
- 适用数据类型：数值型和标称型数据。

处理数据中的缺失值：
- 使用可用特征的均值来填补缺失值
- 使用特殊值来填补缺失值，如-1
- 忽略有缺失值的样本
- 使用相似样本的均值填补缺失值
- 使用另外的机器学习算法预测缺失值

## 支持向量机
- 优点：泛化错误率低，计算开销不大，结果易解释
- 缺点：对参数调节和核函数的选择敏感，原始分类器不加修改仅适用于处理二分类问题
- 适用数据类型：数值型和标称型数据

拉格朗日乘子法

## AdaBoost
- 优点：泛化错误率低，一边吗，可以应用在大部分分类器上，无参数调整
- 缺点：对离群点敏感
- 适用数据类型：数值型和标称型数据
元算法是对其他算他进行组合的一种方式：可以是不同算法的集成，也可以是同一算法在不同设置下的集成。


bagging：基于数据随机重抽样的分类器构建方法

自举汇聚法（boostrap aggregating），从原始数据集中选择S次后得到S个新数据集的一种技术。新数据集和原数据集的大小相等。每个数据集都是通过在院士数据集中随机选择一个样本来进行替换而得到的。这里的替换就意味着可以多次地选择同一样本。这一性质就允许新数据集中可以有重复的值，而原数据集中的某些值在新集合中则不再出现。

在S个数据集建好后，将某个学习算法分别作用于每个数据集就得到了S个分类器。当我们要对新数据进行分类时，就可以应用这S个分类器进行分类。与此同时，选择分类器投票结果中最多的类别作为最后的分类结果。

boosting

不同分类器是通过串行训练而获得的，每个新分类器都根据已训练出的分类器的性能来进行训练。boosting是通过集中关注被已有分类器错分的那些数据来获得新的分类器。由于boosting分类的结果是基于所有分类器的加权求和结果，因此boosting和bagging不太一样。bagging中的分类器权重是相等的，而boosting中的分类器权重并不相等，每个权重代表的是其对应分类器在上一轮迭代中的成功度。
