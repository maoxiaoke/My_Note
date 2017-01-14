# 栈 Stack #
## 栈模型 ##

![栈模型](https://www.tutorialspoint.com/data_structures_algorithms/images/stack_representation.jpg)

### 特点 ###

- 只能在**顶部(top)**处理`push`(进栈)和`pop`(出栈)操作。
- 有时候又被称为**LIFO(先进后出)**表。

## 栈的实现 ##

有两种较为流行的方法：

- **指针实现**
- **数组实现**

### 栈的链表实现 ###

> 第一种实现方法是单链表。在表顶部插入来实现`push`,通过删除表顶端元素实现`pop`,`top`操作只是考查表顶端元素并返回它的值。

![fda ](https://github.com/maoxiaoke/My_Note/blob/master/Algorithm/Pictures/Stack_push.png)