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

使用函数file2matrix读取文件数据，必须确保文件datingTestSet2.txt存储在我们的工作目录中。此外在执行这个函数之前，我们重新加载了kNN.py模块，以确保更新的内容可以生效，否则Python将继续使用上次加载的kNN模块。

成功导入datingTestSet.txt文件中的数据之后，可以简单检查一下数据内容。Python的输出结果大致如下：

```py
>>> diaingDataMat,datingLabels = kNN.file2matrix('datingTestSet2.txt')
>>> diaingDataMat
array([[4.0920000e+04, 8.3269760e+00, 9.5395200e-01],
       [1.4488000e+04, 7.1534690e+00, 1.6739040e+00],
       [2.6052000e+04, 1.4418710e+00, 8.0512400e-01],
       ...,
       [2.6575000e+04, 1.0650102e+01, 8.6662700e-01],
       [4.8111000e+04, 9.1345280e+00, 7.2804500e-01],
       [4.3757000e+04, 7.8826010e+00, 1.3324460e+00]])
>>> datingLabels[0:20]
[3, 2, 1, 1, 1, 1, 3, 3, 1, 3, 1, 1, 2, 1, 1, 1, 1, 1, 2, 3]
>>>
```

现在已经从文本文件中导入了数据，并将其格式化为想要的格式，接着我们需要了解数据的真实含义。当然我们可以直接浏览文本文件，但是这种方法非常不友好，一般来说我们会采用图形化的方式直观地展示数据。下面就用Python工具来图形化展示数据类容，以辨识出一些数据模式。

---

```
                                                  NumPy数组和Python数组

本书将大量使用NumPy数组，你既可以直接在Python命令行环境中输入from numpy import array将其导入，也可以通过直接导入所有NumPy库内容
来将其导入。由于NumPy库提供的数组操作并不支持Python自带的数组类型，因此在编写代码时要注意不要使用错误的数组类型。
```

---



