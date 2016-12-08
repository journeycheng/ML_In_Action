# Machine Learning in Action

Sklearn Matplotlib seaborn Numpy Pandas Scipy


必须考虑两个问题：一、使用机器学习算法的目的，想要算法完成何种任务；二、需要分析或收集的数据是什么

如果想要预测目标变量的值，则可以选择监督学习算法，否则可以选择无监督学习算法。确定选择监督学习算法之后，需要进一步确定目标变量类型，如果目标变量是离散值，则可以选择分类器算法；如果目标变量是连续型的数值，则需要选择回归算法。

如果不想预测目标变量的值，则可以选择无监督学习算法。进一步分析是否需要将数据划分为离散的组。如果这是唯一的需求，则使用聚类算法；如果还需要顾及数据与每个分组的相似程度，则需要使用密度估计算法。

应该充分了解数据，对实际数据了解得越充分，越容易创建复合实际需求的应用程序。主要应该了解数据的以下特性：特征值是离散型变量还是连续型变量，特征值中是否存在缺失的值，何种原因造成缺失值，数据中是否存在异常值，某个特征发生的频率如何，等等。充分了解上面提到的这些数据特性可以缩短选择机器学习算法的时间。

## 一、k-近邻算法
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

[KNN实现识别手写数字](https://github.com/journeycheng/ML_In_Action/blob/master/KNN.md)

## 二、决策树
k近邻算法可以完成很多分类任务，但是它最大的缺点就是无法给出数据的内在含义，决策树的黄祖耀优势就在于数据形式非常容易理解。

- 优点：计算复杂度不高，输出结果易于理解，对中间值的缺失不敏感，可以处理不相关
- 缺点：可能会产生过度匹配的问题
- 使用数据类型：数值型（必须离散化）和标称型

在构造决策树时，需要解决的第一个问题就是，当前数据集上哪个特征在划分数据分类时起决定性作用。为了找到决定性的特征，划分出最好的结果，必须评估每个特征。


遍历当前特征中的所有唯一属性值，对每个特征划分一次数据集，然后计算数据集的新熵值，并对所有唯一特征值的到的熵求和。

熵：H = -sum(p(xi)log_2(p(xi)))

信息增益是熵的减少或者数据无序度的减少

决策树分类器就像带有终止块的流程图，终止块表示分类结果。构建决策树时，通常采用递归的方法将数据集转换为决策树。

其他的决策树的构造算法，最流行的是C4.5和CART

[例子在这里](https://github.com/journeycheng/ML_In_Action/blob/master/DecisionTree.md)
## 三、朴素贝叶斯
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


[Bayes实践案例](https://github.com/journeycheng/ML_In_Action/blob/master/Bayes.md)

## 四、逻辑回归

- 优点：计算代价不高，抑郁理解和实现
- 缺点：容易欠拟合，分类精度可能不高
- 适用数据类型：数值型和标称型数据。

处理数据中的缺失值：
- 使用可用特征的均值来填补缺失值
- 使用特殊值来填补缺失值，如-1
- 忽略有缺失值的样本
- 使用相似样本的均值填补缺失值
- 使用另外的机器学习算法预测缺失值

## 五、支持向量机
- 优点：泛化错误率低，计算开销不大，结果易解释
- 缺点：对参数调节和核函数的选择敏感，原始分类器不加修改仅适用于处理二分类问题
- 适用数据类型：数值型和标称型数据

拉格朗日乘子法

## 六、AdaBoost
- 优点：泛化错误率低，一边吗，可以应用在大部分分类器上，无参数调整
- 缺点：对离群点敏感
- 适用数据类型：数值型和标称型数据
元算法是对其他算他进行组合的一种方式：可以是不同算法的集成，也可以是同一算法在不同设置下的集成。


bagging：基于数据随机重抽样的分类器构建方法

自举汇聚法（boostrap aggregating），从原始数据集中选择S次后得到S个新数据集的一种技术。新数据集和原数据集的大小相等。每个数据集都是通过在院士数据集中随机选择一个样本来进行替换而得到的。这里的替换就意味着可以多次地选择同一样本。这一性质就允许新数据集中可以有重复的值，而原数据集中的某些值在新集合中则不再出现。

在S个数据集建好后，将某个学习算法分别作用于每个数据集就得到了S个分类器。当我们要对新数据进行分类时，就可以应用这S个分类器进行分类。与此同时，选择分类器投票结果中最多的类别作为最后的分类结果。

boosting

不同分类器是通过串行训练而获得的，每个新分类器都根据已训练出的分类器的性能来进行训练。boosting是通过集中关注被已有分类器错分的那些数据来获得新的分类器。由于boosting分类的结果是基于所有分类器的加权求和结果，因此boosting和bagging不太一样。bagging中的分类器权重是相等的，而boosting中的分类器权重并不相等，每个权重代表的是其对应分类器在上一轮迭代中的成功度。

正确率、召回率及ROC曲线


## 七、线性回归
- 优点：结果易于理解，计算上不复杂
- 缺点：对非线性的数据拟合不好
- 适用数据类型：数值型和标称型数据。


回归方程和回归系数


## 八、K-均值聚类

- 优点：容易实现
- 缺点：可能收敛到局部最小值，在大规模数据集上收敛较慢
- 适用数据类型：数值型数据
- 算法流程：首先，随机确定k个初始点作为质心。然后将数据集中的每一个点分配到一个簇中，具体来讲，为每个点找距其最近的质心，并将其分配给该质心所对应的簇。这一步完成之后，每个簇的质心更新为该簇所有点的平均值。直到簇的质心不在改变。
伪代码
```
创建k个点作为起始质心（经常是随机选择）
当任意一点的簇分配结果发生改变时
  对数据集中的每一个数据点
    对每个质心
      计算质心与数据点之间的距离(欧式距离)
    将数据点分配到距其最近的簇
  对每一个簇，计算簇中所有点的均值并将其均值作为质心
```
K-均值算法收敛但聚类效果较差的原因是，K-均值算法收敛到了局部最小值，而非全局最小值。

一种用于度量聚类效果的指标SSE（误差平方和，Sum of Squared Error）。SSE值越小表示数据点越接近它们的质心，聚类效果也越好。因为对误差取了平房，因此更加重视那些远离中心的点。一种肯定可以降低SSE值的方法是增加簇的个数，但这违背了聚类的目标。聚类的目标是在保持簇数目不变的情况下提高簇的质量。

可以对生成的簇进行后处理，一种方法是将具有最大SSE值的簇分成两个簇。具体实现时可以将最大簇包含的点过滤出来并在这些点上运行K-均值算法，其中k设为2.为了保持簇总数不变，可以将某两个簇进行合并。合并最近的质心，或者合并两个使得SSE增幅最小的质心。

### 二分K-均值算法

为了克服K-均值算法收敛于局部最小值的问题，有人提出了另一个称谓二分K-均值的算法。该算法首先将所有点作为一个簇，然后将该簇一分为二。之后选择SSE最大的簇进行划分，直到簇数目达到用户指定的数目为止。

二分K-均值聚类效果要好于K-均值聚类。

## 九、Apriori进行关联分析
- 优点：易编码实现
- 缺点：在大数据集上可能较慢
- 适用数据类型：数值型或者标称型数据

关联分析是一种在大规模数据集中寻找有趣关系的任务。这些关系可以有两种形式：频繁项集或者关联规则。频繁项集是经常出现在一块的物品的集合，关联规则暗示两种物品之间可能存在很强的关系。

Apriori原理是说如果一个元素项是不频繁的，那么那些包含该元素的超集也是不频繁的。Apriori算法从单元素项集开始，通过组合满足最小支持度要求的项集来形成更大的集合。支持度用来度量一个集合在原始数据中出现的频率。

关联分析可以用在许多不同物品上。商店中的商品以及网站的访问页面是其中比较常见的例子。

每次增加频繁项集的大小，Apriori算法都会重新扫描整个数据集。当数据集很大时，这会显著降低频繁项集发现的速度。

## 十、FP-growth算法来高效发现频繁项集

Frequent Pattern

- 优点：一般要快于Apriori
- 缺点：实现比较困难，在某些数据集上性能会下降
- 适用数据类型：标称型数据

FP-growth算法只需要对数据库进行两次扫描。发现频繁项集的基本过程如下：
- 构建FP树
- 从FP树中挖掘频繁项集

## 十一、PCA

对数据进行简化的原因：
- 易于显示
- 使得数据集更易适用
- 降低很多算法的计算开销
- 去除噪声
- 使得结果易懂

在PCA中，数据从原来的坐标系转换到了新的坐标系，新坐标系的选择是由数据本身决定的。第一个新坐标轴选择的是原始数据中方差最大的方向，第二个新坐标轴的选择和第一个坐标轴正交且具有最大方差的方向。该过程一直重复，重复次数为原始数据中特征的数目。大部分方差都包含在最前面的几个新坐标轴重。因此，可以忽略余下的坐标轴，即对数据进行了降维。

- 优点：降低数据的复杂性，识别最重要的多个特征
- 缺点：不一定需要，且可能损失有用信息
- 适用数据类型：数值型数据

通过数据集的协方差矩阵及其特征值分析，就可以求得这些主成分的值。

一旦得到了协方差矩阵的特征向量，可以保留最大的N个向量。这些特征向量也给出了N个最重要特征的真实结构。可以通过将数据乘以N个特征向量，将它转换到新的空间。

Av = \lambda v。v是特征向量，\lambda是特征值。如果特征向量v被被某个矩阵A左乘，那么就等于某个标量\lambda 乘以v。

NumPy中有寻找特征向量和特征值的模块linalg，它有eig()方法，用于求解特征向量和特征值

将数据转换成前N个主成分的伪码如下：
```
 去除平均值
 计算协方差矩阵
 计算协方差阵阵的特征值和特征向量
 将特征值从大到小排序
 保留最上面的N个特征向量
 将数据转换到上述N个特征向量构建的新空间中。 meanRemoved * MainV * MainV.T + meanVals
```
主成分占总方差的百分比。大部分方差都包含在前面的几个主成分中，舍弃后面的主成分并不会损失太多的信息。

降维往往作为与处理步骤，在数据应用到其他算法之前清洗数据。

## 十二、SVD

奇异值分解，Singular Value Decomposition

- 优点：简化数据，去除噪声，提高算法的结果
- 缺点： 数据的转换可能难以理解
- 适用数据类型：数值型数据

利用SVD实现，能够用小很多的数据集来表示原始数据集。这样做，实际上是去除了噪声和冗余信息。

在很多情况下，数据中的一小段携带了数据集中的大部分信息，其他信息则要么是噪声，要么就是毫不相关的信息。

矩阵分解可以将原始矩阵表示成新的易于处理的形式，类似于代数中的因式分解。

```python
U,Sigma,VT = linalg.svd(data)
```
Sigma矩阵只有对角元素，其他元素均为0，一个惯例是Sigma的对角元素是从大到小排列的。这些对角元素称为奇异值。在科学和工程中，一直都存在这样一个普遍事实，在某个奇异值的数目（r个）之后，其他的奇异值都为0.这就意味着数据集中仅有r个重要特征，其余特征都是噪声或冗余特征。

确定要保留的奇异值的数目有很多启发式的策略，其中一个典型的做法就是保留矩阵中90%的能量信息。为了计算总能量信息，将所有的奇异值求平方和，于是可以将奇异值的平方和累加到总值的90%为止。另一个启发策略就是，当矩阵有上万的奇异值时，那么久保留前面2000或3000个。

重构一个原始矩阵的近似矩阵。
```python
Sig3 = mat([Sigma[0], 0, 0], [0, Sigma[1], 0], [0, 0, Sigma[2]])
U[:, :3]*Sig3*VT[:3, :]
```

### 基于协同过滤的推荐引擎


购物网站根据购买历史推荐物品，Netflix向用户推荐电影，新闻网站对用户推荐新闻报道。


Collaborative filtering,协同过滤是通过将用户和其他用户的数据进行对比来实现推荐的。

协同过滤的核心是相似度计算方法；有很多相似度计算方法都可以用于计算物品或用户之间的相似度。通过在低维空间下计算相似度，SVD提高了推荐系统引擎的效果。

比较用户或物品之间的相似度

- 相似度＝1/（1+欧式距离）。当距离为0时，相似度为1；当距离为1时，相似度为0
- 皮尔逊相关系数。度量两个向量之间的相似度，对用户评级的量级并不敏感（所有的5分和1分，皮尔逊系数会认为相等）。0.5+0.5\*corrcoef()将（－1，1）归一化到（0，1）
- 余弦相似度，计算两个向量夹角的余弦值。如果夹角为90度，相似度为0，如果两个向量的方向相同，则相似度为1. cos \theta = A\* B/(|A||B|) 同样需要将（－1，1）归一化到（0，1）

推荐引擎的评价：采用交叉测试的方法。将某些已知的评分值去掉，然后对它们进行预测，然后计算预测值和真实值之间的差异。

评价指标：最小均方根误差（Root Mean Squared Error, RMSE）.首先计算均方误差的平均值后取其平方根。如果评级在1星到5星这个范围内，RMSE为1.0，那么意味着我们的预测值和用户给出的真实评价相差了一个星级。

推荐系统的工作过程是：给定一个用户，系统会为此用户返回N个最好的推荐菜，为了实现这一点，需要做到：
- 寻找用户没有评级的菜肴，即在用户－物品矩阵中的0值
- 在用户没有评级的所有物品中，对每个物品预计一个可能的评级分数。这就是说，我们认为用户可能会对物品的打分（这就是相似度计算的初衷）
- 对这些物品的评分从高到低进行排序，返回前N个物品

[SVD的例子](https://github.com/journeycheng/ML_In_Action/blob/master/SVD.md)

构建推荐引擎面临的挑战
- 矩阵的表示方法，实际系统中有大量的0: 可以通过只存储非零元素来节省内存和计算开销
- 计算资源的浪费。每次需要一个推荐得分时，都需要计算多个物品的相似度得分，这些纪录可以被另一个用户重复使用。一个普遍的解决做法是离线计算并保存相似度得分
- 冷启动问题。如何在缺乏数据时给出好的推荐。通过各种标签来标记，基于内容的推荐。

Content-Based Recommendations

一般包含以下三个过程：
- Item Representation: 为每个item抽取一些特征来表示此item
- Profile Learning：利用一个用户过去喜欢（及不喜欢）的item的特征数据，来学习出此用户的喜好特征（profile）
- Recommendataion Generation: 通过比较上一步得到的用户Profile与候选item的特征，为此用户推荐一组相关性最大的item

朴素贝叶斯、K-近邻、决策树

CB的优点：
- 用户之间的独立性（User Independent）：既然每个用户的profile都是依据他本身对item的喜好获得的，自然就与他人的行为无关。而CF刚好相反，CF需要利用很多其他人的数据。CB的这种用户独立性带来的一个显著好处是不管别人如何作弊（比如利用多个账号把某个产品的排名刷上去）都不会影响到自己
- 好的可解释性（Transparency）：如果需要向用户解释为什么推荐了这些产品给他，你只要告诉他这些产品有某某属性，这些属性跟你的品味很匹配。
- 新的item可以立刻得到推荐（New Item Problem）：只要一个新item加进item库，它就马上可以被推荐，被推荐的机会和老item是一致的。而CF对于新item就恨无奈，只有当此新item被某些用户喜欢过（或被打过分），它才可能被推荐给其他用户。所以如果一个纯CF的推荐系统，新加进来的item就永远不会被推荐。


CB的缺点：
- item的特征抽取一般很难(Limited Content Analysis)。
- 无法挖掘出用户的潜在兴趣（Over-specialization）。既然CB的推荐只依赖于用户过去对某些item的喜欢，它产生的推荐也都会和用户过去喜欢的item相似。
- 无法为新用户产生推荐（New User Problem）：新用户没有喜好历史，自然无法获得他的profile，所以也就无法为她产生推荐了。这个问题CF也有。


### 基于SVD的图像压缩
假设原始图像的大小是32\*32＝1024像素（二值化处理），使用SVD对数据降维，从而实现图像的压缩。如果只需要2个奇异值就能相当精确地对图像实现重构，那么就只需要32\*2 + 2 + 2\*32=130个数字进行存储。

SVD是一种强大的降维工具，可以利用SVD来逼近矩阵并从中提取重要特征。通过保留矩阵80%～90%的能量，就可以得到重要的特征并去掉噪声。


