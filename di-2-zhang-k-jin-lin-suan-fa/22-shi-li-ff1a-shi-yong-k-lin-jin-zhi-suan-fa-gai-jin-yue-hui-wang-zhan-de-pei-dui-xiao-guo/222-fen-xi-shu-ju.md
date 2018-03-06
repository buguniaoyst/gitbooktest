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

![](/assets/Figure_1.png)

```
                                     玩视频游戏所耗时间百分比
```

```
                       图2-3 没有样本类别标签的约会散点图。难以辨识途中的点究竟属于哪个样本分类
```

由于没有使用样本分类的特征值，我们很难从图2-3中看到任何有用的数据模式信息。一般来说，我们会采用色彩或其他的记号来标记不同样本分类，以便更好地理解数据信息。Matplotlib库提供的scatter函数支持个性化标记散点图上的点。重新输入上面的代码，调用scatter函数时使用下列参数：

```py
>>> ax.scatter(datingDataMat[:,1],datingDataMat[:,2],15.0*array(datingLabels),15.0*array(datingLabels))
<matplotlib.collections.PathCollection object at 0x000001FE8D94B080>
>>> plt.show()
>>>
```

上述代码利用变量datingLabels存储的类标签属性，在散点图上绘制了色彩不等，尺寸不同的点。你可以看到一个与图2-3类似的散点图。从图2-3中，我们很难看到任何有用的信息，然而由于图2-4利用颜色及尺寸表示了数据点的属性类别，因而我们基本上可以从图2-4中看到数据点所属三个样本分类的区域轮廓。

![](/assets/Figure_2.png)

                                                               玩视频游戏所耗时间百分比



