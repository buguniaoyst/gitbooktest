# 使用Python导入数据

首先，创建名为kNN.py的Python模块，在构造完整的k-近邻算法之前，我们还需要编写一些基本的通用函数，在kNN.py中增加以下代码：

`from numpy import *`

`import operator`

`def createDataSet();`

`group = array([ [1.0,1.1],[1.0,1.0],[0,0],[0,0.1]])`

`labels = ['A','A','B','B']`

`return group,labels`

在上面的代码中，我们导入了两个模块：第一个是科学计算包NumPy;第二个是运算符模块，k-近邻算法执行排序操作时，将使用这个模块提供的函数。

为了方便使用createDataSet\(\)函数，它创建数据集和标签，如图2-1所示。然后依次执行以下步骤：保存kNN.py文件的位置，打开Python开发环境，进入Python的交互模式，输入一下命令导入上面编辑的程序模块：

    import kNN

