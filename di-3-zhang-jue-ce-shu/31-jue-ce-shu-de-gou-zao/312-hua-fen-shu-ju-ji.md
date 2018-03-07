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



