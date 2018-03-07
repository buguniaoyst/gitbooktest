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
        print("the classifier came back with: %d,the real answer is:%d"\
            (classifierResult,classNumStr))
        if(classifierResult != classNumStr):errorCount += 1.0
    print ("\nthe total number of errors is: %d" % errorCount)
    print ("\n the total error rate is:%f" % (errorCount/float(mTest))
```



