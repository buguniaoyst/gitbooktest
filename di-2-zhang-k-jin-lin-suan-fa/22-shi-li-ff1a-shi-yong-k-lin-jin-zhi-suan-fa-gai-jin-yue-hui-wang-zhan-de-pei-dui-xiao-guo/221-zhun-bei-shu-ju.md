# 准备数据：从文本文件中解析数据

我们的数据都存储在datingTestSet2.txt中，每个样本数据占据一行，总共有1000行。这些样本数据主要包含以下3中特征：

* 每年获得的飞行常客里程数。
* 玩视频游戏所耗费的时间百分比。
* 每周消费的冰淇淋公升数。

在上述特征数据输入到分类器之前，必须将待处理数据的格式改变为分类器可以接受的额格式。在kNN.py中创建名为file2matrix的函数，以此来处理输入格式问题。该函数的输入为文件名字符串，输出为训练样本矩阵和类标签向量。

将下面的代码增加到kNN.py中。

---

程序清单2-2 讲文本记录转换为NyumPy的解析程序

```py
def file2matrix(filename):
    fr = open(filename)
    arrayOLines = fr.readlines()
    numberOfLines = len(arrayOLines)
    returnMat = zeros((numberOfLines,3))
    classLabelVector = []
    index = 0
    for line in arrayOLines:
        line = line.strip()
        listFromLine = line.split('\t')
        returnMat[index,:] = listFromLine[0:3]
        classLabelVector.append(int(listFromLine[-1]))
        index += 1
    return returnMat,classLabelVector
```

从上面的代码可以看出，Python处理文本文件非常容易。首先我们需要知道文本文件有多少行。打开文件，得到文件的行数。然后创建以零填充的矩阵NumPy（实际上，NumPy是一个二维数组，这里暂时不用考虑其他用途）。为了简化处理，我们将该矩阵的另一维度设置为固定值3，你可以按照自己的实际需求增加相应的代码以适应变化的输入值。循环处理文件中的每行数据，首先使用函数line.strip\(\)截取掉所有的回车符，然后使用tab字符\t将上一步得到的证行数据分割成一个元素列表。接着，我们选取前3个元素，将他们存储到特征矩阵中。Python语言可以使用所应知-1表示列表中的最后一行元素，利用这种负索引，我们可以很方便地将列表的最后一列存储到向量classLabelVector中。需要注意的是，我们必须明确地通知解释器，告诉它列表中存储的元素值为整型，否则Python会把这些元素当作字符串处理。以前我们必须自己处理这些变量值类型问题，现在这些细节问题完全可以交给NumPy函数库来处理。

在Python命令提示符下输入下面命令：

```py
reload(kNN)

datingDataMat,datingLabels = kNN.file2matrix('datingTestSet2.txt')
```

使用函数file2matrix读取文件数据，必须确保文件datingTestSet2.txt存储在我们的工作目录中。此外在执行这个函数之前，我们重新加载了kNN.py

