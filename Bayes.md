## 使用朴素贝叶斯进行文档分类

要从文本中获得特征，需要先拆分文本，然后将每一个文本片段表示为一个词条向量，其中值为1表示词条出现在文档中，0表示词条未出现。

词表到向量的转换函数

```python
def loadDataSet():
    postingList=[['my', 'dog', 'has', 'flea', 'problems', 'help', 'please'],
                 ['maybe', 'not', 'take', 'him', 'to', 'dog', 'park', 'stupid'],
                 ['my', 'dalmation', 'is', 'so', 'cute', 'I', 'love', 'him'],
                 ['stop', 'posting', 'stupid', 'worthless', 'garbage'],
                 ['mr', 'licks', 'ate', 'my', 'steak', 'how', 'to', 'stop', 'him'],
                 ['quit', 'buying', 'worthless', 'dog', 'food', 'stupid']]
    classVec = [0,1,0,1,0,1]    #1 is abusive, 0 not
    return postingList,classVec
```
- 返回的第一个变量是进行词条切分后的文档集合，这些文档来自于斑点狗爱好者留言板。文本被切分成一些列的词条集合，标点符号从文本中去掉。
- 返回的第二个变量是上述文档的类别标签集合

------

创建一个包含在所有文档中出现的不重复词的词汇列表
```python
def createVocabList(dataSet):  # 创建两个集合的并集
    vocabSet = set([])
    for document in dataSet:
        vocabSet = vocabSet | set(document)
    return list(vocabSet)
```

------

将文档映射到词汇列表中，用0、1向量表示
```python
def setOfWords2Vec(vocabList, inputSet):
    returnVec = [0]*len(vocabList)
    for word in inputSet:
        if word in vocabList:
            returnVec[VocabList.index(word)] = 1
        else:
            print 'the word: %s is not in my Vocabulary!' % word
    return returnVec
```

------

------

先测试一下：
```python
>>> listPosts, listClasses = loadDataSet()
>>> myVocabList = createVocabList(listPosts)
>>> myVocabList
['cute', 'love', 'help', 'garbage', 'quit', 'I', 'problems', 'is', 'park', 'stop', 'flea', 'dalmation', 'licks', 'food', 'not', 'him', 'buying', 'posting', 'has', 'worthless', 'ate', 'to', 'maybe', 'please', 'dog', 'how', 'stupid', 'so', 'take', 'mr', 'steak', 'my']
>>> setOfWords2Vec(myVocabList, listPosts[0])
[0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1, 1, 0, 0, 0, 0, 0, 0, 1]
```
