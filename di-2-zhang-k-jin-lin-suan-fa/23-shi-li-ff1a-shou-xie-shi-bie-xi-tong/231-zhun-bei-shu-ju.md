# 准备数据：将图像转换为测试向量

实际图像存储在资料中的两个目录中：目录trainingDigists中包含了大约2000个例子，每个例子的内容如图2-6所示，每个数字大约有200个样本；目录testDigits中包含了大约900个测试数据。我们使用目录trainingDigits中的数据训练分类器，使用目录testDigits中的数据测试分类器的效果。

![](/assets/手写识别1.png)

图2-6 手写数字数据集的例子

为了使用前面两个例子的分类器，我们必须将图像格式化处理为一个向量。我们将把一个32\*32的二进制图像矩阵转换为1\*1024的向量，这样前两节使用的分类器就可以处理数字图像信息了。

我们首先编写一段函数img2vector，将图像转换为向量：该函数创建1\*1024的NumPy数组，然后打开给定的文件，循环独处文件的前32行，并将每行的头32个字符值存储在NumPy数组中，最后返回数组。

代码清单-将图像转为向量

```py
def img2vector(filename):
    returnVect = zeros((1,1024))
    fr = open(filename)
    for i in range(32):
        lineStr = fr.readline()
        for j in range(32):
            returnVect[0,32*i+j] = int(lineStr[j])
    return returnVect
```

执行以下命令，可得到转换后的向量结果：

```py
>>> import kNN
>>> testVector = kNN.img2vector('testDigits/0_13.txt')
>>> testVector[0,0:31]
array([0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 1., 1., 1.,
       1., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0.])
>>> testVector[0,32:63]
array([0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 1., 1., 1., 1., 1.,
       1., 1., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0.])
>>>
```



