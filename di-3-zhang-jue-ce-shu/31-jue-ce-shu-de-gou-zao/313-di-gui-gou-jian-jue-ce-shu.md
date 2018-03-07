# 递归构建决策树

目前我们已经学习了从数据集构造决策树算法所需要的子功能模块，其工作原理如下：得到原始数据集，然后基于最好的属性值划分数据集，由于特征值可能多余两个，因此可能存在大于两个分支的数据集划分。第一次划分后。书记江北向下传递到树分支的下一个节点，在这个节点上，我们可以再次划分数据。因此我们可以采用递归的原则处理数据集。

递归结束的条件是：程序遍历完所有划分数据集的属性，或者每个分支下的所有实例都具有相同的分类。如果所有实例具有相同的分类，则得到一个叶子节点或者终止块。任何到达叶子节点的数据必然属于叶子节点的分类，参见图3-2所示。

![](/assets/递归构建决策树.png)图3-2 划分数据集时的数据路径

第一个结束条件使得算法可以终止，我们甚至可以设置算法可以划分的最大分组数目。后续章节还会介绍其他决策树算法，如C4.5和CART，这些算法在运行时并不总是在每次划分分组时都会消耗特征。由于特征数目并不是在每次划分数据分组时都减少，因此这些算法在实际使用时可能会引起一定的问题。目前我们并不需要考虑这个问题，只需要在算法开始运行前计算列的数目，查看算法是否使用了所有属性即可。如果数据集已经树立了所有属性，但是累标签依然不是唯一的，此时我们需要决定如何定义该叶子节点，在这种情况下，我们通常会采用多数表决的方法决定该叶子结点的分类。

打开文本编辑器，在增加下面的函数之前，在trees.py文件顶部增加一行代码：import operator,然后添加下面的代码到trees.py文件中：

```py
def majorityCnt(classList):
    classCount = {}
    for vote in classList:
        if vote not in classCount.keys():classCount[vote] = 0
        classCount[vote] += 1
    sortedClassCount = sorted(classCount.iteritems(),key=operator.itemgetter(1),reverse=True)
    return sortedClassCount[0][0]
```

上面的代码与第二章classify0部分的投票表决代码非常类似，该函数使用分类名称的列表，然后创建键值为classList中唯一值的数据字典，字典对象存储了classList中每个标签出现的频率，最后利用operator操作键值排序字典，并返回出现次数最多的分类名称。

在文本编辑器中打开trees.py文件，添加下面的程序代码。

程序清单3-4 创建树的函数代码

```py
def createTree(dataSet,labels):
    classList = [example[-1] for example in dataSet]
    if classList.count(classList[0]) == len(classList):
        return classList[0]
    if len(dataSet[0]) == 1:
        return majorityCnt(classList)
    bestFeat = chooseBestFeatureToSplit(dataSet)
    bestFeatLabel = labels[bestFeat]
    myTree = {bestFeatLabel:{}}
    del(labels[bestFeat])
    featValues = [example[bestFeat] for example in dataSet]
    uniqueVals = set(featValues)
    for value in uniqueVals:
        subLabels = labels[:]
        myTree[bestFeatLabel][value] = createTree(splitDataSet(dataSet,bestFeat,value),subLabels)
    return myTree
```

程序清单3-4的代码使用两个输入参数：数据集合标签列表。标签列表包含了数据集中所有特征的标签，算法本身并不需要这个变量，但是为了给出数据明确的含义，我们将他作为一个输入参数提供。此外，前面提到的对数据集的要求依然满足。上述代码首先创建了名为classList的列表变量，其中包含了数据集的所有标签。递归函数的第一个停止条件是所有的类标签完全相同，则直接返回该类标签。递归函数的第二个停止条件是使用完了所有特征，仍然不能将数据集划分成仅包含唯一类别的分组。由于第二个条件无法简单滴返回唯一的类别标签，这里使用程序清单3-3的函数挑选出现次数最多的类别作为返回值。

下一步程序开始创建树，这里使用Python语言的字典类型存储树的信息，当然也可以声明特殊的数据类型存储树，但是这里完全没有必要。字典变量myTree存储了树的所有信息，这对于其后绘制树形图非常重要。当前数据集选取的最好特征存储在变量bestFeat中，得到列表包含的所有属性值。这部分代码与程序清单3-3中的部分代码类似，这里就不再进一步解释了。

最后代码遍历当前选中特征包含的所有属性值，在每个数据集划分上递归调用函数createTree\(\)，得到的返回值将被插入到字典变量myTree中，因此函数终止执行时，字典中将会潜逃很多代表叶子节点信息的字典数据。在解释这个字典数据之前，我们先看一下循环的第一行subLabels = labels\[:\]，这行 代码复制了类标签，并将其存储在新列表变量subLabels中。之所以这样做，是因为在Python语言中函数参数是列表类型时，参数是按照引用方式传递的。为了保证每次调用函数createTree\(\)时不改变原始列表的内容，使用新变量subLabels代替原始列表。

现在我们可以测试上面代码的实际输出结果，执行下面的命令：

```py
>>> reload(trees)
<module 'trees' from 'G:\\Python\\test\\决策树\\trees.py'>
>>> myData,labels = trees.createDataSet()
>>> myTree = trees.createTree(myData,labels)
>>> myTree
{'no surfacing': {0: 'no', 1: {'flippers': {0: 'no', 1: 'yes'}}}}
>>>
```

变量myTree包含了很多代表树结构信息的嵌套字典，从左边开始，第一个关键字no surfacing是第一个划分数据集的特征名称，关键字的值也是另一个数据字典。第二个关键字是no surfacing特征划分数据集，这些关键字的值是no surfacing节点的子节点。这些值可能是类标签，也可能是另一个数据字典。如果值是类标签，则该子节点是叶子节点；如果值是另一个数据字典，则子节点是一个判断节点，这种格式结构不断重复就构成了整棵树。本节的例子中，这棵树包含了3个叶子结点以及2个判断节点。



