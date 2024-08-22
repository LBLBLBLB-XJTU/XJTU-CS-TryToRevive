# 神经网络

- [神经网络](#----)
    * [神经网络](#-----1)
    * [激活函数](#----)
    * [神经网络结构](#------)
    * [梯度下降](#----)
    * [多层神经网络](#------)
    * [BP网络](#bp--)
    * [过度拟合](#----)
    * [TensorFlow](#tensorflow)
    * [CV](#cv)
    * [图像卷积](#----)
    * [卷积神经网络](#------)
    * [循环神经网络](#------)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

## 神经网络

神经网络是由神经科学的发展而激发而来的.在大脑中,神经元之间互相连接.每个神经元都可以收到或发送电信号.当神经元收到的电信号超过,某个阈值时,神经元被激活然后继续向前发送电信号

人工神经网络是对神经网络的模拟模型.人工神经网络的函数将基于模型的结构和参数,将输入映射到输出.人工神经网络中的结构和参数着由对于数据的训练产生.

在AI中实现时,每个神经元时并行的连接到其他单元的.例如杜预输入$x_1,x_2$,可以构建模型为:$h(x_1, x_2) = w_0 + w_1x_1 + w_2x_2$.其中$w_1,w_2$为权重,$w_0$为偏差

## 激活函数

为了确定所谓的超过某个阈值,神经元被激活这个过程,我们使用激活函数来实现.这个激活函数有多种形式:

* 阶跃函数:$g(x) =\begin{cases} 0 &\text{if }x<0  \\1 & \text{if } x \geq 0 \end{cases}  $,小于0时值为0,大于等于0时,值为1
* 逻辑函数:$g(x) = \frac{e^x}{e^x +1}$,值是0到1之间的实数,表达了对判断的信心
* 修正线性单元(ReLU):$g(x)=max(0,x)$,大于等于0时输出可以是任意正值,小于0时输出为0

无论激活函数是什么,输入都要加上偏差

## 神经网络结构

神经网络可以被视为上述思想的代表,其中的函数将输入相加以产生输出

![神经网络结构](https://github.com/LBLBLBLB-XJTU/XJTU-CS-TryToRevive/blob/main/resource/nnstructure.png)

图中,左边的两个白色单元是输入，右边的单元是输出.输入通过加权边连接到输出.为了做出决定,输出单元将输入乘以它们的权重以及偏差(w₀),并使用函数g来确定输出.

假定$w_1,w_2,x_0$分别为1,1,-1,则可以表示为$g(-1 + 1x_1 + 1x_2)$,当使用阶跃函数为激活函数时,当自变量大于等于0,输出为1,否则为0

## 梯度下降

梯度下降算法是为了在训练神经网络时最小化loss.利用这个算法,我们能够基于训练数据来计算模型中的权重,算法如下:

* 以随机权重开始
* 重复:
  * 计算所有点的梯度,形成一个梯度向量
  * 根据梯度向量更新权重

这个方法的问题在于需要计算所有点的权重,计算量大.为了减少这个计算量,有很多方法,例如:***随机梯度下降***:梯度由随机选择的点产生.但是这种会因为随机而正确率下降,因此有了***小批量梯度下降***:由随机选择一些点来计算梯度.当然,这些算法都不是完美的,每个有其对应的场景

由这些算法,我们可以确定模型的权重,来解决对应的问题

至此我们的网络依赖于感知器输出单元,只能画出线性决定边界.然而这个正确边界往往不是线性的,需要新的办法

## 多层神经网络

多层神经网络是是由一个输入层,一个输出层,以及若干隐藏层的网络.我们训练时只会讲数据提供给输入层和输出层,而不会给隐藏层.在第一隐藏层的神经元,将会收到输入层的每个单元的输出的加权和作为输入,执行一些操作后输出一个值,这个值又将作为下一层的输入的一部分,直到输出层.使用这些隐藏层,可以作出非线性的模型

## BP网络

反向传播网络(BP网络),是对含有隐藏的神经网络进行训练的主流算法.算法由输出单元的误差开始,计算前一层权重的梯度下降,重复这个过程,直到输入层.伪代码如下:

* 计算输出层误差
* 对于每一层,从输出层开始并向内移动到最早的隐藏层:
  * 将误差传播一层.换句话说,当前层将误差发送到前一层
  * 更新权重

这个过程可以适用于任意层的网络,来创建深度网络(具有多于一层隐藏层的网络)

## 过度拟合

过度拟合是模型过于贴近训练数据,而失去了对于新数据的通用性.解决这个问题的一个方法是dropout.在这种方法中,会在学习过程中暂时性的随机移除节点.这样我们能够阻止对于某个节点的过度依赖.在整个训练过程中,神经网络将采取不同的形式,每次都会丢弃一些其他单元,然后再次使用它们

注意,在训练结束后,神经网络仍是完整的

## TensorFlow

python中的TensorFlow库已经提供了如BP算法的实现.

## CV

计算机视觉包含了多种方法来理解和分析图片.

图片由像素组成,每个像素由三个0到255的值组成,分别是红,绿,蓝,即RGB.由此可以创建一个神经网络,以像素为输入,中间有一些隐藏层,输出是一些值,告诉我们图像中显示了什么.这个方法欧一些缺点,首先,把图像打成了像素,不能利用图片的结构.其次,输入的个数过多,意味着需要大量计算权重

## 图像卷积

图像卷积应用了一个过滤器,将每个像素与周围的像素组合(利用矩阵(kernel)加权),来有助于神经网络的处理

这个组合是将选择到的原图像的矩阵值和kernel中的对应值相乘,然后再相加.

对于不同任务,我们使用不同的内核.对于边缘检测,通常使用这个内核:$\left[ \begin{array}{ccc} -1&-1&-1 \\ -1&8&-1 \\ -1&-1&-1 \end{array} \right]$.

其中的思想是:当像素与邻居相似时,则应相互抵消,结果为0.因此,像素越相似,图像部分就越暗,越不同,则越亮

图像卷积的实现如下:(使用PIL库):

```
import math
import sys

from PIL import Image, ImageFilter

# Ensure correct usage
if len(sys.argv) != 2:
    sys.exit("Usage: python filter.py filename")

# Open image
image = Image.open(sys.argv[1]).convert("RGB")

# Filter image according to edge detection kernel
filtered = image.filter(ImageFilter.Kernel(
    size=(3, 3),
    kernel=[-1, -1, -1, -1, 8, -1, -1, -1, -1],
    scale=1
))

# Show resulting image
filtered.show()
```

然而,由于输入的像素众多,计算量仍然很大.另一个解决的方法是池化,输入的大小将被减少,输出将会从区域中采样.相邻的像素将视为比较相似的,通过采样即可取出作为代表.其中的一个方法是***最大池化***,选中的像素是区域中值最大的

## 卷积神经网络

卷积神经网络是使用了卷积的神经网络,常常用于分析图片.网络先利用不同功能的过滤器通过卷积来提取图片的特征.这些过滤器也可以通过BP来优化.然后进行池化,池化后输入到传统的神经网络中(即“平坦化”)

卷积和池化可以进行多次来提取特征和减少输入大小.这样做的好处是可以降低对与图片的敏感度,即一张图片,改变一个小角度后,仍将返回近似的结果.

卷积和池化不会相对于传统神经网络大规模的改变代码.TensorFlow提供了黑白手写数字作为数据集,我们用一下代码来识别数字:





```
import sys
import tensorflow as tf

# Use MNIST handwriting dataset
mnist = tf.keras.datasets.mnist

# Prepare data for training
(x_train, y_train), (x_test, y_test) = mnist.load_data()
x_train, x_test = x_train / 255.0, x_test / 255.0
y_train = tf.keras.utils.to_categorical(y_train)
y_test = tf.keras.utils.to_categorical(y_test)
x_train = x_train.reshape(
    x_train.shape[0], x_train.shape[1], x_train.shape[2], 1
)
x_test = x_test.reshape(
    x_test.shape[0], x_test.shape[1], x_test.shape[2], 1
)

# Create a convolutional neural network
model = tf.keras.models.Sequential([

    # Convolutional layer. Learn 32 filters using a 3x3 kernel
    tf.keras.layers.Conv2D(
        32, (3, 3), activation="relu", input_shape=(28, 28, 1)
    ),

    # Max-pooling layer, using 2x2 pool size
    tf.keras.layers.MaxPooling2D(pool_size=(2, 2)),

    # Flatten units
    tf.keras.layers.Flatten(),

    # Add a hidden layer with dropout
    tf.keras.layers.Dense(128, activation="relu"),
    tf.keras.layers.Dropout(0.5),

    # Add an output layer with output units for all 10 digits
    tf.keras.layers.Dense(10, activation="softmax")
])

# Train neural network
model.compile(
    optimizer="adam",
    loss="categorical_crossentropy",
    metrics=["accuracy"]
)
model.fit(x_train, y_train, epochs=10)

# Evaluate neural network performance
model.evaluate(x_test,  y_test, verbose=2)
```

运行程序,给程序一个手写数字,即可对数字识别.

## 循环神经网络

至此我们讨论的都是前馈神经网络,输入值进入输入层,线性的经过网络输出到输出层.

而循环神经网络与此相反,为一个非线形的结构,网络的输出将作为输入

循环神经网络适用于序列化的数据
