## 决策树

测试用的数据集
```python
def createDataSet():
    dataSet = [[1, 1, 'yes'],
               [1, 1, 'yes'],
               [1, 0, 'no'],
               [0, 1, 'no'],
               [0, 1, 'no']]
    labels = ['no surfacing','flippers']
    return dataSet, labels
```


计算给定数据集的香农熵
```python
from math import log
def clacShannonEnt(dataSet):
    numEntries = len(dataSet)
    labelCounts = {}
    for featureVec in dataSet:
        currentLabel = featureVec[-1]
        if currentLabel not in labelCounts.keys():
            labelCounts[currentLabel] = 0
        labelCounts[currentLabel] += 1
    shannonEnt = 0.0
    for key in labelCounts:
        prob = float(labelCounts[key])/numEntries
        shannonEnt -=prob * log(prob, 2)
    return shannonEnt
```
熵越高，混合的数据也越多


```python
>>> myDat, labels = createDataSet()
>>> calcShannonEnt(myDat)
0.9709505944546686
```

划分数据集

将符合要求的元素抽取出来

参数：待划分的数据集、划分数据集的特征（序号）、特征的值
```python
from math import log
def splitDataSet(dataSet, axis, value):
    retDataSet = []
    for featureVec in dataSet:
        if featureVec[aixs] == value:
            reducedFeatVec = featureVec[:axis]
            reducedFeatVec.extend(featureVec[axis+1:])
            retDataSet.append(reducedFeatVec)
    return retDataSet
```

提取第一列值为1的部分
```python
>>> splitDataSet(myDat, 0, 1)
[[1, 'yes'], [1, 'yes'], [0, 'no']]
```


按照信息增益最大的条件进行划分
```python
def chooseBestFeatureToSplit(dataSet):
    numFeatures = len(dataSet[0]) - 1
    baseEntropy = calcShannonEnt(dataSet)
    bestInfoGain = 0.0
    bestFeature = -1
    
    for i in range(numFeatures):
        featList = [example[i] for example in dataSet]
        uniqueVals = set(featList)
        newEntropy = 0.0
        for value in uniqueVals:
            subDataSet = splitDataSet(dataSet, i, value)
            prob = len(subDataSet)/float(len(dataSet))
            newEntropy += prob * calcShannonEnt(subDataSet)
        
        infoGain = baseEntropy - newEntropy
        
        if (infoGain > bestInfoGain):
            bestInfoGain = infoGain
            bestFeature = i
    return bestFeature
```


```python
>>> chooseBestFeatureToSplit(myDat)
0
```

递归构建决策树算法

得到原始数据集，然后基于最好的属性值划分数据集，由于特征值可能多于两个，因此可能存在大于两个分支的数据集划分。第一次划分之后，数据被向下传递到树分支的下一个节点，在这个节点上，可以再次划分数据。因此，可以采用递归的原则处理数据集。

递归结束的条件是：程序遍历完所有划分数据集的属性，或者每个分支下的所有实例都具有相同的分类。

```python 
import operator
def majorityCnt(calssList):
    classCount={}
    for vote in classList:
        if vote not in classCount.keys():
            classCount[vote] = 0
        classCount[vote] += 1
        
    sortedClassCount = sorted(classCount.iteritems(), key=operator.itemgetter(1), reverse=True)
    return sortedClassCount[0][0]
```

```python
def createTree(dataSet, labels):
    classList = [example[-1] for example in dataSet]
    
    if classList.count(classList[0] == len(classList)):  # 类别完全相同则停止继续划分
        return classList[0]
    if len(dataSet[0]) == 1:     # 遍历完所有特征时返回出现次数最多的
        return majorityCnt(classList)
        
    bestFeat = chooseBestFeatureToSplit(dataSet)
    bestFeatLabel = labels[bestFeat]
    
    myTree = {bestFeatLabel:{}}
    del(labels[bestFeat])
    
    featValues = [example[bestFeat] for example in dataSet]
    uniqueVals = set(featValues)
    for value in uniqueVals:
        subLabels = labels[:]
        myTree[bestFeatLabel][value] = createTree(splitDataSet(dataSet, bestFeat, value), subLabels)
    return myTree
```

```python
>>> myTree = createTree(myDat, labels)
>>> myTree
{'no surfacing': {0: 'no', 1: {'flippers': {0: 'no', 1: 'yes'}}}}
```

使用决策树的分类函数
```python
def classify(inputTree, featLabels, testVec):
    firstStr = inputTree.keys()[0]
    secondDict = inputTree[firstStr]
    featIndex = featLabels.index(firstStr)
    for key in secondDict.keys():
        if testVec[featIndex] == key:
            if type(secondDict[key]).__name__ == 'dict':
                classLabel = classify(secondDict[key], featLabels, testVec)
            else:
                classLabel = secondDict[key]
    return classLabel
```

```python
>>> classify(myTree, labels, [1, 0])
'no'

构造
