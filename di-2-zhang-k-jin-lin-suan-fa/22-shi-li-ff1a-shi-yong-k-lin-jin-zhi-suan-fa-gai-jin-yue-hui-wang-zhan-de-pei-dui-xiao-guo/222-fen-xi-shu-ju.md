# 数据分析：使用Matplotlib创建散点图

首先我们使用Matplotlib制作原始数据散点图，使用Matplotlib库之前需要先安装。

## 下载Matplotlib库

下载地址：[https://pypi.python.org/pypi/matplotlib](https://pypi.python.org/pypi/matplotlib)

下载对应版本的Matplotlib

## 安装Matplotlib库

在cmd中输入：pip install D:\python3.6.1\Scripts\[matplotlib-2.2.0-cp36-cp36m-win\_amd64.whl

## 使用Matplotlib库创建散点图

安装完成后，在Python命令行环境中，输入下列命令：

```py
>>> import kNN
>>> datingDataMat,datingLabels = kNN.file2matrix('datingTestSet2.txt')
>>> import matplotlib
>>> import matplotlib.pyplot as plt
>>> fig = plt.figure()
>>> ax = fig.add_subplot(111)
>>> ax.scatter(datingDataMat[:,1],datingDataMat[:,2])
<matplotlib.collections.PathCollection object at 0x0000019BD5871240>
>>> plt.show()
>>>
```

输出结果如图2-3所示。散点图使用datingDataMat矩阵的第二，第三列数据，分别表示特征值“玩视频游戏所耗时间百分比”和“每周所消费的冰淇淋公升数”。

