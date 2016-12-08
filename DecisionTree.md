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

得到
