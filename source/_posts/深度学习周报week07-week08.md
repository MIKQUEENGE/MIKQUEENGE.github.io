---
title: 深度学习周报week07-week08
toc: true
date: 2018-05-19 09:49:31
categories:
- deep learning
tags:
---

## 音频分类

将1000个音频分别放入对应文件夹中：

![](/images/classification.PNG)

`filename.txt` 存储文件对应位置。

以上所有存储在`classification`文件夹中。

建立`filename.py`来生成`filename.txt`：

```python
import os

textname = 'filename.txt'
with open(textname, 'w') as f:
    for root, dirs, afiles in os.walk('./classification'):
        for subdir in dirs:
            for subroot, subdirs, subfiles in os.walk('./classification/'+subdir):
                for filename in subfiles:
                    apath = os.path.join(subdir, filename)
                    f.write(apath)
                    f.write('\n')
```

生成的`filename.txt`为：

![](/images/filename_txt.PNG)

## MFCC

[学习链接](https://blog.csdn.net/fengzhonghen/article/details/51722555)

**MFCC**(Mel-frequency cepstral coefficients)：梅尔频率倒谱系数。

梅尔频率是基于人耳听觉特性提出的概念， 它与Hz频率成非线性对应关系。

MFCC则是利用它们之间的这种关系，计算得到的Hz频谱特征，<u>主要用于语音数据特征提取和降低运算维度</u>。

<u>主要用于语音数据特征提取和降低运算维度。</u>

例如：对于一帧有512维(采样点)数据，经过MFCC后可以提取出最重要的40维(一般而言)数据同时也达到了将维的目的。

MFCC的步骤为：

- 预加重
- 分帧
- 加窗
- **快速傅里叶变换(FFT)**
- **梅尔滤波器组**
- 离散余弦变换(DCT)

其中最重要的就是FFT和梅尔滤波器组，这两个进行了主要的降维操作。

## 了解相关库和函数

- scipy.io.wavfile
- python_speech_features
- librosa
- pydub
- tf.sparse_tensor_to_dense
- tf.edit_distance
- tf.nn.ctc_loss