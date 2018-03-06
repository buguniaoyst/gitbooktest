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



