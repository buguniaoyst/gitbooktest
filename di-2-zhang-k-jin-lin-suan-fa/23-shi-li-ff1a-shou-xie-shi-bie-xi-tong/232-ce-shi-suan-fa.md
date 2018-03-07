# 测试算法：使用k-近邻算法识别手写数字

上节我们已经将数据处理成分类器可以识别的格式，本节我们将这些数据输入到分类器，检测分类器的执行效果。程序清单2-6所示的包含子函数handwritingClassTest\(\)是测试分类器的代码，将其写入kNN.py文件中。在写入这些代码之前，我们必须确保将from os import listdir写入文件的其实部分，这段代码的主要功能是从os模块中导入函数listdir，它可以列出给定目录的文件名。

程序清单2-6

```py
def handwritingClassTest():
    hwLabels = []
    trainingFileList = listdir('trainingDigits')
    m = len(trainingFileList)
    trainingMat = zeros((m,1024))
    for i in range(m):
        fileNameStr = trainingFileList[i]
        fileStr = fileNameStr.split('.')[0]
        classNumStr = int(fileStr.split('_')[0])
        hwLabels.append(classNumStr)
        trainingMat[i,:] = img2vector('trainingDigits/%s' % fileNameStr)
    testFileList = listdir('testDigits')
    errorCount = 0.0
    mTest = len(testFileList)
    for i in range(mTest):
        fileNameStr = testFileList[i]
        fileStr = fileNameStr.split('.')[0]
        classNumStr = int(fileStr.split('_')[0])
        vectorUnderTest = img2vector('testDigits/%s' % fileNameStr)
        classifierResult = classify0(vectorUnderTest,trainingMat,hwLabels,3)
        print((classifierResult,classNumStr))
        if(classifierResult != classNumStr):errorCount += 1.0
    print ("\nthe total number of errors is: %d" % errorCount)
    print ("\n the total error rate is:%f" % (errorCount/float(mTest))
```

在程序清单2-6中，将trainingDigits目录中的文件内容存储在列表中，然后可以得到目录中有多少文件，并将其存储在变量m中。接着，代码创建一个m行1024列的训练矩阵，该矩阵的每行数据存储一个图像。我们可以从文件名中解析出分类数字。该目录下的文件名按照规则命名，如文件9\_45.txt的分类是9，它是数字9的45个实例。然后我们可以将类代码存储在hwLabels向量中，使用前面讨论的img2vector函数载入图像。在下一步中，我们对testDigits目录中的文件执行相似的操作，不同之处是我们并不将这个目录下的文件载入矩阵中，而是使用classify0\(\)函数测试该目录下的每个文件。由于文件中的值已经在0和1之间，本节并不需要使用到autoNorm\(\)函数。

在Python命令提示符中输入kNN。handwritingClassTest\(\)，测试该函数的输出结果。依赖于机器速度，加载数据及可能需要花费很长时间，然后函数开始依次测试每个文件，输出结果如下所示：

```py
>>> import kNN
>>> kNN.handwritingClassTest()
```

执行结果如下：

```py
(0, 0)
(0, 0)
(0, 0)
......
(9, 9)
(9, 9)
(9, 9)

the total number of errors is: 10

 the total error rate is:0.010571
>>>
```



