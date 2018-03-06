# 使用算法：构建完整可用系统

上面我们已经在数据上对分类器进行了测试，现在终于可以使用这个分类器为海伦对人们进行分类。我们会给海伦一小段程序，通过该程序，海伦会在约会网站上找到某个人并输入他的信息。程序会给出她对对方喜欢程度的预测值。

将以下代码加入到kNN.py中。

```py
# 程序清单2-5 约会网站预测函数
def classifyPerson():
    resultList = ['not at all','in small doses','in large doses']
    percentTats = float(input("percentage of time spent playing video games?"))
    ffMiles = float(input("frequent flier miles earned per year?"))
    iceCream = float(input("liters of ice cream consumed per year?"))
    datingDataMat,datingLabels = file2matrix('datingTestSet2.txt')
    normMat,ranges,minVals = autoNorm(datingDataMat)
    inArr = array([ffMiles,percentTats,iceCream])
    classifilerResult = classify0((inArr-minVals)/ranges,normMat,datingLabels,3)
    print(resultList[classifilerResult-1])
```

输入一下命令：

```py
>>> kNN.classifyPerson()
percentage of time spent playing video games?10
frequent flier miles earned per year?10000
liters of ice cream consumed per year?0.5
in small doses
```

目前为止，我们已经看到如何在数据上构建分类器。

