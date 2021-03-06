# 划分数据集

上节我们学习了如何度量数据集的无序程度，分类算法除了要测量信息熵，还需要划分数据集，度量花费数据集的熵，以便判断当前是否正确地划分了数据集。我们将对每个特征划分数据集的计算结果计算一次信息熵，然后判断按照那个特征划分数据集是最好的划分方式。想象一个分布在二维空间的数据散点图，需要在数据之间画条线，将他们分成两部分，我们应该按照x轴还是y轴划线呢？答案就在本节讲述的内容中。

要划分数据集，打开文本编辑器，在trees.py文件中输入下列代码：

程序清单3-2 按照给定特征划分数据集

```py
def splitDataSet(dataSet,axis,value):
    retDataSet = []
    for featVec in dataSet
        if featVec[axis] == value:
            reducedFeatVec = featVec[:axis]
            reducedFeatVec.extend(featVec[axis+1:])
            retDataSet.append(reducedFeatVec)
    return retDataSet
```

程序清单3-2的代码使用了三个输入参数：待划分的数据集，划分数据集的特征，特征的返回值。需要注意的是，Python语言不用考虑内存分配的问题。Python语言在函数中传递的是列表的引用，在函数内部对列表对象的修改，将会影响该列表对象的整个生存周期。为了消除这个不良影响，我们需要在函数的开始声明一个新列表对象。因为该函数代码在同一数据集上被调用多次，为了不修改原始数据集，创建一个新的列表对象。数据集这个列表中的各个元素也是列表，我们要遍历数据集中的每个元素，一旦发现符合要求的值，将其添加到新创建的列表中。在if语句中，程序将符合特征的数据抽取出来。后面讲述的更简单，这里我们可以这样理解这段代码：当我们按照某个特征划分数据集时，就需要将所有符合要求的元素抽取出来。代码中使用了Python语言列表里类型自带的extent\(\)和append\(\)方法。这两个方法功能类似，但是在处理多个列表时，这两个方法的处理结果是完全不同的。

假定存在两个列表，a和b：

```py
>>> a=[1,2,3]
>>> b=[4,5,6]
>>> a.append(b)
>>> a
[1, 2, 3, [4, 5, 6]]
```

如果执行a.append\(b\)，则列表得到了第四个元素，而且第四个元素也是一个列表。然而如果使用extend方法：

```py
>>> a=[1,2,3]
>>> a.extend(b)
>>> a
[1, 2, 3, 4, 5, 6]
```

则得到一个包含a和b所有元素的列表。

我们可以在前面的简单样本数据上测试函数splitDataSet\(\)。首先还是要讲程序清单3-2的代码增加到trees.py文件中，然后在Python命令提示符内输入下述命令：

```py
>>> import trees
>>> myData,labels = trees.createDataSet()
>>> myData
[[1, 1, 'yes'], [1, 1, 'yes'], [1, 0, 'no'], [0, 1, 'no'], [0, 1, 'no']]
>>> trees.splitDataSet(myData,0,1)
[[1, 'yes'], [1, 'yes'], [0, 'no']]
>>> trees.splitDataSet(myData,0,0)
[[1, 'no'], [1, 'no']]
```

接下来我们将遍历整个数据集，循环计算香农熵和spliDataSet\(\)函数，找到最好的特征划分方式。熵计算将会告诉我们如何划分数据集时最好的数据组织方式。

打开文本编辑器，在trees.py文件中输入下面的程序代码。

程序清单3-3 选择最好的数据集划分方式

```py
def chooseBestFeatureToSplit(dataSet):
    numFeatures = len(dataSet[0]) - 1
    baseEntropy = calcShannonEnt(dataSet)
    bestInfoGain = 0.0; bestFeature = -1
    for i in range(numFeatures):
        featList = [example[i] for example in dataSet]
        uniqueVals = set(featList)
        newEntropy = 0.0
        for value in uniqueVals:
            subDataSet = splitDataSet(dataSet,i,value)
            prob = len(subDataSet)/float(len(dataSet))
            newEntropy += prob * calcShannonEnt(subDataSet)
        infoGain = baseEntropy = newEntropy
        if(infoGain > bestInfoGain):
            bestInfoGain = infoGain
            bestFeature = i
    return bestFeature
```

程序清单3-3给出了函数chooseBestFeatureToSplit\(\)的完整代码，该函数实现选取特征，划分数据集，计算得出最好的划分数据集的特征。函数chooseBestFeatureToSplit\(\)使用了程序清单3-1和程序清单3-2中的函数。在函数中调用的数据需要满足一定的要求：第一个要求是，数据必须是一种由列表元素组成的列表，而且所有的列表元素都要具有相同的数据长度；第二个要求是，数据的最后一列或者每个实例的最后一个元素是当前实例的类别标签。数据集一旦满足上述要求，我们就可以在函数的第一行判定当前数据集包含多少特征属性。我们无需限定list中的数据类型，它们既可以是数字也可以是字符串，并不影响实际计算。

在开始划分数据集之前，程序清单3-3的第3行代码计算了整个数据集的原始香农熵，我们保存 最初的无序度量值，用于与划分完之后的数据集计算的熵值进行比较。第1个for循环遍历数据集中的所有特征。使用列表推导（List Comprehension）来创建新的列表，将数据集中所有第i个特征值或者所有可能存在的值写入这个新list中。然后使用Python语言原生的集合（set）数据类型。集合数据类型与列表数据类型相似，不同之处仅在于集合类型中的每个值互不相同。从列表中创建集合时Python语言得到列表中唯一元素值的最快方法。遍历当前特征中的所有唯一属性值，对每个特征划分一次数据集，然后计算数据集中的新熵值，并对所有唯一特征值，对每个特征划分一次数据集，然后计算数据集的新熵值，并对所有唯一特征值得到的熵求和。信息增益是熵的减少或者数据无需度的减少，大家肯定对于将熵用于度量数据无序的减少更容易理解。最后，比较所有特征中的信息增益，返回最好特征划分的索引值。

现在我们可以测试上面代码的实际输出结果，首先将程序清单3-3的内容输入到文件trees.py中，然后在Python命令提示符下输入下列命令：

```py
>>> from imp import reload
>>> reload(trees)
<module 'trees' from 'G:\\Python\\test\\决策树\\trees.py'>
>>> myData,labels = trees.createDataSet()
>>> trees.chooseBestFeatureToSplit(myData)
0
>>> myData
[[1, 1, 'yes'], [1, 1, 'yes'], [1, 0, 'no'], [0, 1, 'no'], [0, 1, 'no']]
>>>
```

代码运行结果告诉我们，第0个特征是最好的用于划分数据集的特征。结果是否正确呢？这个结果又有什么实际意义呢？数据集中的数据来源于表3-1，让我们回头在看一下表1-1或者变量myData中的数据。如果我们按照第一个特征属性划分数据，也就是说第一个特征是1的放在一个组，第一个特征是0的放在另一个组，数据一致性如何？按照上述的方法划分数据集，第一个特征为1的海洋生物分组将有两个属于鱼类，一个属于非鱼类；另一个分组则全部属于非鱼类。如果按照第二个特征分组，结果又是怎样呢？

