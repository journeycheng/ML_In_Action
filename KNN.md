## 基于KNN的手写识别系统

用到的库
```python
from numpy import *
import operator
from os import listdir
```


kNN算法：4个输入参数，用于分类的输入向量inX，输入的训练样本集dataSet，标签向量labels，选择最近邻的数目k。
```python
def classify0(inX, dataSet,labels, k):
    dataSetSize = dataSet.shape[0]  
    diffMat = tile(inX, (dataSetSize, 1)) - dataSet # 将inx扩展成和dataSet同样的数量，用于矩阵计算
    sqDiffMat = diffMat**2  # 矩阵的点平方
    sqDistances = sqDiffMat.sum(axis=1)  # 矩阵按行累加
    distances = sqDistances**0.5  # 开根号，得到欧式距离
    sortedDistIndicies = distances.argsort() # 排序
    
    classCount = {}
    for i in range(k):
        voteIlabel = labels[sortedDistIndicies[i]]
        classCount[voteIlabel] = classCount.get(voteIlabel, 0) + 1
    sortedClassCount = sorted(classCount.iteritems(), key=operator.itemgetter(1), reverse=True)
    return sortedClassCount[0][0]
```

需要识别的数字已经使用图形处理软件，处理成具有相同的色彩和大小（32\*32），存储为txt文件，内容大致是这样的。
```
00000000000011110000000000000000
00000000000011111100000000000000
00000000000111111110000000000000
00000000001111111111100000000000
00000000011111111111000000000000
00000000011111111111100000000000
00000000011111111111000000000000
00000000011111111111000000000000
00000000111111111110000000000000
00000000111111111110000000000000
00000000111111111100000000000000
00000000011111111100000000000000
00000000111111111000000000000000
00000000011111111100000000000000
00000000111111111000000000000000
00000000011111111000000000000000
00000000011111111000000000000000
00000000111111111000000000000000
00000000011111111000000000000000
00000000011111111000000000000000
00000000011111111000000000000000
00000000111111111000000000000000
00000000011111111000000000000000
00000000011111111000000000000000
00000000001111111000000000000000
00000000011111111000000000000000
00000000001111111100000000000000
00000000001111111000000000000000
00000000000111111100000000000000
00000000000111111000000000000000
00000000000011111100000000000000
00000000000011111000000000000000
```

读取文本的内容，转换成一个向量。
```python
def img2vector(filename):
    returnVect = zeros((1, 1024))
    fr = open(filename)
    for i in range(32):
        lineStr = fr.readline()
        for j in range(32):
            returnVect[0, 32*i + j] = int(lineStr[j])
    return returnVect
```

导入训练集和测试集数据，并进行KNN：
```python
def handwritingClassTest():
    hwLabels = []
    traningFileList = listdir('trainingDigits')
    m = len(trainingFileList)
    traingMat = zeros((m, 1024))
    for i in range(m):
        fileNameStr = traingingFileList[i]
        fileStr = fileNameStr.split('.')[0]
        classNumStr = int(fileStr.split('_'))[0]
        hwLabels.append(classNumStr)
        traingMat[i, :] = img2vector('trainingDigits/%s' % fileNameStr)
        
        
    testFileList = listdir('testDigits')
    errorCount = 0.0
    mTest = len(testFileList)
    for i in range(mTest):
        fileNameStr = testFileList(i)
        fileStr = fileNameStr.split('.')[0]
        classNumStr_check = int(fileStr.split('_'))[0]
        vectorOfTest = img2vector('testDigits/%s' % fileNameStr)
        classfierResult = classify0(vectorOfTest, trainingMat, hwLabels, 3)
        
        print 'the classifier came back with: %d, the real answer is: %d' % (classifierResult, classNumStr_check)
        
        if (classifierResult != classNumStr):
            errorCount += 1.0
    print '\nthe total number of errors is: %d' % errorCount
    print '\nthe total error rate is: %f' % (errorCount/float(mTest))
```

进行检验
```python
>>> handwritingClassTest()
the classifier came back with: 0, the real answer is: 0
the classifier came back with: 1, the real answer is: 1
...
the classifier came back with: 9, the real answer is: 9

the total number of predicting errors is: 12

the total error rate is: 0.012672
```


k近邻算法必须保存全部数据集，如果训练数据集很大，必须使用大量的存储空间。此外，由于必须对数据集中的每个数据计算距离，实际使用时可能非常耗时。

另一个缺陷是它无法给出任何数据的基础结构信息，因此无法知晓平均实例样本和典型实例样本具有什么特征。


将图片根据灰度值转换为0、1序列
```python
from PIL import Image
from numpy import *

def img2num(filename):
    im = Image.open(filename)
    im = im.resize((32, 32), Image.NEAREST)
    im = im.convert('RGB')
    
    pixdata = im.load()
    imgVect = zeros((1,1024))
    for y in range(im.size[1]):
        for x in range(im.size[0]):
            Gray = (pixdata[x, y][0]*0.3 + pixdata[x, y][1]*0.59 + pixdata[x, y][2]*0.11)
            if Gray < 150:
                imgVect[0, 32*y+x] = 1
    return imgVect
```

```python
def handwritingClassTest():
    hwLabels = []
    trainingFileList = listdir('trainingDigits')           #load the training set
    m = len(trainingFileList)
    trainingMat = zeros((m,1024))
    for i in range(1,m):
        fileNameStr = trainingFileList[i]
        fileStr = fileNameStr.split('.')[0]     #take off .txt
        classNumStr = int(fileStr.split('_')[0])
        hwLabels.append(classNumStr)
        trainingMat[i,:] = img2vector('trainingDigits/%s' % fileNameStr)
        
    imgVect = img2num('test1.jpg')
    classifierResult = classify0(imgVect, trainingMat, hwLabels, 3)
    print "the classifier came back with: %d" % classifierResult
```

```python
>>> handwritingClassTest()
the classifier came back with: 1
```
