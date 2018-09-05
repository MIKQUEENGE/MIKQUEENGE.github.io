---
title: TensorFlow训练MNIST报错ResourceExhaustedError
date: 2018-04-01 12:35:44
categories: 
- deep learning
tags:
- MNIST
- TensorFlow
---



在最后测试的一步报错：

```shell
ResourceExhaustedError (see above for traceback): OOM when allocating tensor
```

搜索了一下才知道是GPU显存不足（emmmm....）造成的，可以把最后测试的那行代码改为将测试集分成几个小部分分别测试最后再求精度的平均值：

```python
accuracy_sum = tf.reduce_sum(tf.cast(correct_prediction, tf.float32))
good = 0
total = 0
for i in range(10):
    testSet = mnist.test.next_batch(50)
    good += accuracy_sum.eval(feed_dict={ x: testSet[0], y_: testSet[1], keep_prob: 1.0})
    total += testSet[0].shape[0]
print ("test accuracy %g"%(good/total))
```

