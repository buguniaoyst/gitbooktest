# 如何选择合适的算法

从上一节中列出的算法中选择实际可用的算法，必须考虑下面两个问题：一，使用机器学习算法的目的，想要算法完成何种任务，比如是预测明天下雨的概率还是对投票者按照兴趣分组；二，需要分析或收集的数据是什么。

首先考虑使用机器学习算法的目的。如果想要预测目标变量的值，则可以选择监督学习算法，否则可以选择无监督学习算法。确定选择监督学习算法之后，需要进一步确定目标变量的类型，如果目标变量是离散型，如是/否，1/2/3,A/B/C或者红/黄/黑等，则可以选择分类算法；如果目标变量是连续型的数据，如0.0~100.0,-999~999，则需要选择回归算法。

如果不想预测目标变量的值，则可以选择无监督学习算法。进一步分析是否需要将数据划分为离散的组。如果这是唯一的需求，则使用聚类算法；如果还需要估计数据与每个分组的相似程度，则需要使用密度估计算法。

其次需要考虑的是数据问题。我们应该充分了解数据，对实际数据了解得越充分，越容易创建符合实际需求的应用程序。主要应该了解数据的以下特征：特征值是离散型变量还是连续型变量，特征值中是否存在缺失的值，何种原因造成缺失值，数据中是否存在异常值，某个特征发生的频率如何（时候罕见得如同海底捞针），等等。充分了解上面提到的这些数据特征可以缩短选择机器学习算法的时间。

我们只能在一定程度上缩小算法的选择范围，一般不存在最好的算法或者可以给出最好结果的算法，同时还要尝试不同算法的执行结果。对于所选的每种算法，都可以使用其他的机器学习技术来改进其性能。在处理输入数据之后，两个算法的相对性能也可能会发生变化。一般说来发现最好算法的关键环节是反复试错的迭代过程。

机器学习算法虽然各不相同，但是使用算法创建应用程序的不走却基本类似。















注：

离散型数据：

离散数据是指其数值只能用自然数或整数单位计算的数据。例如：企业个数、职工人数、设备台数等，只能按计量单位数计数。这种数据的数值一般用计数方法取得。在统计学中，数据按变量值是否连续可分为连续数据与离散数据两种。

