# 实施kNN分类算法

本节使用程序清单2-1的函数运行kNN算法，为每组数据分类。这里首先给出k-近邻算法的伪代码和实际的Python代码，然后详细地址解释每行代码的含义。该函数功能是使用k-近邻算法将每组数据划分到某个类中，其伪代码如下：

对位置类别属性的数据集中的每个点一次执行以下操作：

（1）计算一直类别数据集中的点与当前点之间的距离；

（2）按照距离递增次序排序；

（3）选取与当前点距离最小的k个点；

（4）确定前k个点所在类别的出现频率；

（5）返回前k个点出现频率最高的类别作为当前点的预测分类；

Python函数classify0\(\)程序清单2-1所示：

```py
#程序清单2-1  k-近邻算法
from numpy import *
import operator

def createDataSet():
    group = array([[1.0,1.1],[1.0,1.0],[0,0],[0,0.1]])
    labels = ['A','A','B','B']
    return group, labels

def classify0(inX,dataSet,labels,k):
    dataSetSize = dataSet.shape[0] #dataSet.shape[0] 得到数组的行数
    diffMat = tile(inX,(dataSetSize,1))-dataSet #
    sqDiffMat = diffMat**2
    sqDistances = sqDiffMat.sum(axis=1)
    distances = sqDistances**0.5
    sortedDistIndicies = distances.argsort()
    classCount = {}
    for i in range(k):
        voteIlabel = labels[sortedDistIndicies[i]]
        classCount[voteIlabel] = classCount.get(voteIlabel,0) + 1
    sortedClassCount = sorted(classCount.items(),
        key=operator.itemgetter(1),reverse=True)
    return sortedClassCount[0][0]
```

![](/assets/欧氏距离计算公式.png)

![](/assets/欧氏距离计算公式3.png)

计算完所有点之间的距离后，可以对 数据按照从小到大的次序排序。然后，确定前k个距离最小元素所在的主要分类，输入k总是正整数；最后，将classCount字典分解为元组列表，然后使用程序第二行导入运算符模块itemgetter方法，按照第二个元素的次序对元组进行排序。此处的排序为逆序，即按照从最大到最小次序排序，最后返回发生频率最高的元素标签。

为了预测数据所在分类，在Python提示符中输入下列命令：

    kNN.classify0\(\[0,0\],group,labels,3\)



