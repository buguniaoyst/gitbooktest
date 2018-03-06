# 测试算法：作为完整程序验证分类器

上节我们已经将数据按照需求做了处理，本节我们将测试分类器的效果，如果分类器的正确率满足要求，海伦就可以使用这个软件来处理约会网站提供的名单了。机器学习算法一个很重要的工作就是评估算法的正确率，通常我们只提供了已有数据的90%作为训练样本来训练分类器，而使用其余的10%数据区测试分类器，检测分类器的正确率。

对于分类器来说，错误率就是分类器给出错误结果的次数除以测试数据的总数，代码里我们定义了一个计数器变量，每次分类器错误地分类数据，计数器就会加1，程序执行完成之后计数器的结果除以数据点总数即是错误率。

为了测试分类器效果，在kNN.py文件中创建函数datingClassTest,该函数是自包含的，你可以在任何时候在Python运行环境中使用该函数测试分类器的效果。在kNN.py文件中输入下面的程序代码。

```py
# 程序清单2-4 分类器针对约会网站的测试代码def datingClassTest():
	hoRatio = 0.10
	datingDataMat,datingLabels = file2matrix('datingTestSet2.txt')
	normMat,ranges,minVals = autoNorm(datingDataMat)
	m = normMat.shape[0]
	numTestVecs = int(m*hoRatio)
	errorCount = 0.0
	for i in range(numTestVecs):
		classifierResult = classify0(normMat[i,:],normMat[numTestVecs:m,:],datingLabels[numTestVecs:m],3)
		print("the classifier came back with:%d,the real answer is:%d % (classifierResult,datingLabels[i])")
		if (classifierResult != datingLabels[i]):errorCount += 1.0 
	print("the total error rate is:%f" % (errorCount/float(numTestVecs)))
```



