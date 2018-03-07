# 信息增益

划分数据集的大原则是：将无需的数据变得更加有序。我们可以使用多种方法划分数据集，但是每种方法都有各自的优缺点。组织杂乱无章数据的一种方法就是使用信息论度量信息，信息论是量化处理信息的分支科学。我们可以在划分数据之前使用信息论量化度量信息的内容。

在划分数据集之前之后信息发生的变化成为信息增益，知道如何计算信息增益，我们就可以计算每个特征值划分数据集获得的信息增益，获得信息增益最高的特征就是最好的选择。

在可以评测哪种数据划分方式是最好的数据划分之前，我们必须学习计算如何计算信息增益。集合信息的度量方式成为香农熵或者简称为熵，这个名字来源于信息论之父克劳德.香农。

如果看不明白什么是信息增益和熵，请不要着急----它们自诞生的那一天起，就注定令世人十分费解。克劳德.香农看完信息论之后，约翰.冯.诺依曼建议使用“熵”这个术语，因为大家不知道它是什么意思。

熵定义为信息的期望值，在明晰这个概念之前，我们必须知道信息的定义。如果带分类的事务可能划分在多个分类之中，则xi的信息定义为：

![](/assets/信息增益.png)

![](/assets/信息增益2.png)其中n是分类的数目。

下面我们将学习如何使用Python计算信息熵，创建名为trees.py的文件，将程序清单3-1的代码内容录入到trees.py文件中，此代码的功能是计算给定数据集的熵。

程序清单3-1 计算给定数据集的香农熵

```py
from math import log

def calcShannonEnt(dataSet):
    numEntries = len(dataSet)
    lebelCounts = {}
    for featVec in dataSet:
        currentLabel = featVec[-1]
        if currentLabel not in labelCounts.keys():
            labelCounts[currentLabel] = 0
            labelCounts[currentLabel] += 1
        shannonEnt = 0.0
        for key in labelCounts:
            prob = float(labelCounts[key])/numEntries
            shannonEnt -= prob * log(prob,2)
        return shannonEnt
```

程序清单3-1的代码非常简单。首先，计算数据集中实例的总数。我们也可以在需要时再计算这个值，但是由于代码中多次用到这个值，为了提高代码效率，我们显式地声明一个变量保存实例总数。然后，创建一个数据字典，它的键值是最后一列的数值。如果当前键值不存在，则扩展字典并将当前键值加入字典。每个键值都记录了当前类别出现的次数。最后，使用所有类标签的发生频率计算类别出现的频率。我们将这个概率计算香农熵，统计所有类标签发生的次数。下面我们看看如何使用熵划分数据集。

在trees.py文件中，我们可以利用createDataSet\(\)函数得到列表3-1所示的简单鱼鉴定数据集，你可以输入自己的createDataSet\(\)函数：

```py
def createDataSet():
	dataSet = [
				[1,1,'yes'],
				[1,1,'yes'],
				[1,0,'no'],
				[0,1,'no'],
				[0,1,'no']
				]
	labels = ['no surfacing','flippers']
	return dataSet,labels
```



