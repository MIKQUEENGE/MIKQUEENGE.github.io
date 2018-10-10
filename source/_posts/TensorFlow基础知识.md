---
title: TensorFlow基础知识
date: 2018-03-31 14:13:12
categories: 
- deep learning
tags:
- TensorFlow
---



**TensorFlow**是一个采用**数据流图（data flow graphs）**，用于数值计算的开源软件库。

<!-- more -->

下图就是一个**数据流图**。数据流图是一个用来描述数学计算的由**“结点”（nodes）和“线”(edges)**组成的**有向图**。

**“节点”** 一般用来表示施加的数学操作，但也可以表示数据输入（feed in）的起点或者是输出（push out）的终点，以及读取/写入持久变量（persistent variable）的终点。

**“线”**表示“节点”之间的输入/输出关系。这些数据“线”可以输运**“张量”（tensor）**，即大小可动态调整的多维数据数组。

一旦输入端的所有张量准备好，节点将被分配到各种计算设备完成异步并行地执行运算。

![Tensors Flowing](http://www.tensorfly.cn/images/tensors_flowing.gif)

# 开始学习

[官方基础知识链接](http://www.tensorfly.cn/tfdoc/get_started/basic_usage.html)

- 使用图 (graph) 来表示计算任务.
- 在被称之为 `会话 (Session)` 的上下文 (context) 中执行图.
- 使用 tensor 表示数据.
- 通过 `变量 (Variable)` 维护状态.
- 使用 feed 和 fetch 可以为任意的操作(arbitrary operation) 赋值或者从其中获取数据.
- 图中的节点被称之为 *op* (operation 的缩写). 一个 op 获得 0 个或多个 `Tensor`, 执行计算,
  产生 0 个或多个 `Tensor`. 每个 Tensor 是一个类型化的多维数组.

## 构建图（创造节点）

```python
import tensorflow as tf

# 创建一个常量 op, 产生一个 1x2 矩阵. 这个 op 被作为一个节点
# 加到默认图中.
#
# 构造器的返回值代表该常量 op 的返回值.
matrix1 = tf.constant([[3., 3.]])

# 创建另外一个常量 op, 产生一个 2x1 矩阵.
matrix2 = tf.constant([[2.],[2.]])

# 创建一个矩阵乘法 matmul op , 把 'matrix1' 和 'matrix2' 作为输入.
# 返回值 'product' 代表矩阵乘法的结果.
product = tf.matmul(matrix1, matrix2)
```

## 启动图

```python
# 启动默认图.
sess = tf.Session()

# 调用 sess 的 'run()' 方法来执行矩阵乘法 op, 传入 'product' 作为该方法的参数. 
# 上面提到, 'product' 代表了矩阵乘法 op 的输出, 传入它是向方法表明, 我们希望取回
# 矩阵乘法 op 的输出.
#
# 整个执行过程是自动化的, 会话负责传递 op 所需的全部输入. op 通常是并发执行的.
# 
# 函数调用 'run(product)' 触发了图中三个 op (两个常量 op 和一个矩阵乘法 op) 的执行.
#
# 返回值 'result' 是一个 numpy `ndarray` 对象.
result = sess.run(product)
print result
# ==> [[ 12.]]

# 任务完成, 关闭会话.
sess.close()
```

也可以：

```python
with tf.Session() as sess:
  result = sess.run([product])
  print result
```

## Tensor

TensorFlow 程序使用 tensor 数据结构来代表所有的数据, 计算图中, 操作间传递的数据都是 tensor.
可以把 TensorFlow tensor 看作是一个 n 维的数组或列表. 一个 tensor 包含一个静态类型 rank, 和
一个 shape. 

## 变量

变量维护图执行过程中的状态信息。

启动图后, 变量必须先经过`初始化` (init) op 初始化,

必须增加一个`初始化` op 到图中：`init_op = tf.initialize_all_variables()`

启动图后首先运行 'init' op：`sess.run(init_op)`

## Fetch

为了取回操作的输出内容, 可以在使用 `Session` 对象的 `run()` 调用 执行图时, 传入一些 tensor来取回结果。

## Feed

TensorFlow 还提供了 feed 机制, 该机制可以临时替代图中的任意操作中的 tensor,可以对图中任何操作提交补丁, 直接插入一个 tensor。

最常见的用例是将某些特殊的操作指定为 "feed" 操作,标记的方法是使用 tf.placeholder() 为这些操作创建占位符. 

```python
input1 = tf.placeholder(tf.types.float32)
input2 = tf.placeholder(tf.types.float32)
output = tf.mul(input1, input2)

with tf.Session() as sess:
  print sess.run([output], feed_dict={input1:[7.], input2:[2.]})
```

