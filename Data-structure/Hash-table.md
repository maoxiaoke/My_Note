# 散列

> 散列是一种用于以常数平均时间执行**插入**、**删除**和**查找**的技术。 但是，那些需要元素间任何排序信息的操作将不会得到有效的支持。

散列表/哈希表是根据键（`Key`）而直接访问在内存存储位置的数据结构。也就是说，它通过计算一个关于键值的函数，将所需查询的数据映射到表中一个位置来访问记录，这加快了查找速度。这个映射函数称做**散列函数**，存放记录的数组称做**散列表**。

举个例子：

为了查找电话簿中某人的号码，可以创建一个按照人名首字母顺序排列的表（即建立人名`x`到首字母`F(x)`的一个函数关系），在首字母为`W`的表中查找“王”姓的电话号码，显然比直接查找就要快得多。这里使用人名作为**关键字**，“取首字母”是这个例子中散列函数的函数法则`F()`，存放首字母的表对应**散列表**。

> 以上来源参考：[https://zh.wikipedia.org/wiki/%E5%93%88%E5%B8%8C%E8%A1%A8](https://zh.wikipedia.org/wiki/%E5%93%88%E5%B8%8C%E8%A1%A8)
