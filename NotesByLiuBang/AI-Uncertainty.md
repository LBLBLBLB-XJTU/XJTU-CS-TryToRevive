# 不确定性
有时AI并不能获取全部的信息来得出百分百准确的结论。但是AI仍然能够通过信息来作出最优的决定

- [不确定性](#----)
  * [可能性](#---)
    + [possible worlds可能性世界](#possible-worlds-----)
    + [可能性公理](#-----)
    + [无条件概率](#-----)
  * [条件概率](#----)
  * [随机变量](#----)
    + [独立性](#---)
  * [贝叶斯公式](#-----)
  * [联合概率](#----)
  * [概率公式](#----)
  * [贝叶斯网络](#-----)
  * [推理](#--)
    + [枚举推理](#----)
  * [采样](#--)
    + [似然权重](#----)
  * [马尔可夫模型](#------)
    + [马尔可夫假设](#------)
    + [马尔可夫链](#-----)
  * [隐式马尔可夫模型](#--------)
    + [传感器马尔可夫假设](#---------)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

## 可能性
不确定性可以被事件及事件发生的可能性来表示
### possible worlds可能性世界
每一种可能的情况，可以被是为一个world，用ω表示。其中某一个world出现的概率，以P(ω)来表示
### 可能性公理
- 0<P(ω)<1：可能性的值处于0和1之间
  - 0代表不可能发生的事件
  - 1代表确定发生
  - 一般而言，数值越大，发生的概率越高
- 所有可能的事件的可能性的和为1
- 事件的可能性即是这个world的数量/所有world的数量
### 无条件概率
无条件概率是不考虑任何其他证据时对一个命题的相信程度

## 条件概率
条件概率是在给定一些已经证据的情况下对某个命题的相信程度，如我们上面所言，AI可以依据部分信息进行推断，为了进行这样的操作，我们就依赖于条件概率

条件概率以如下表示：P(a | b)，意为在b发生的条件下，a发生的概率。为了计算条件概率，有以下公式  
![条件概率公式](https://github.com/LBLBLBLB-XJTU/XJTU-CS-TryToRevive/blob/main/resource/conditional.png)  
用语言来讲，在b发生的情况下a发生的概率，等于a和b同时发生的概率除b发生的概率。进一步的，这个公式可以被扩展为以下两条：
![条件概率公式扩展](https://github.com/LBLBLBLB-XJTU/XJTU-CS-TryToRevive/blob/main/resource/conditionalequivalent.png)

## 随机变量
随机变量在概率论中的一个变量，具有可能取值的域。例如，可以定义一个航班随机变量，取值为{准点，延误，取消}

我们用概率分布来表示每个值发生的可能性，在上面的例子中有：
- P(Flight = on time) = 0.6
- P(Flight = delayed) = 0.3
- P(Flight = canceled) = 0.1

意为准点的概率为0.6，延误的概率为0.3，取消的概率为0.1。可见，这几项的和为1
### 独立性
独立性是指：一个事情发生的概率不影响另一饿过事情发生的概率。例如，同时掷两个骰子，每一个骰子的结果互相独立。
与此相反的则是相关事件，如云彩的出现会影响下雨的概率。

独立性可如下定义：当且仅当P(a ∧ b) = P(a)P(b)，则a和b独立

## 贝叶斯公式
贝叶斯公式用于计算条件概率，贝叶斯公式表明：在a成立的条件下，b的概率等于，在b成立的条件下a的概率，乘上b的概率除a的概率  
![贝叶斯公式](https://github.com/LBLBLBLB-XJTU/XJTU-CS-TryToRevive/blob/main/resource/bayesrule.png)  
那么，通过P(a | b)和P(a) and P(b), 能够得到P(b | a)。这意味着，在拥有P(visible effect | unknown cause)时，我们就可以得到P(unknown cause | visible effect)

## 联合概率
联合概率是多个事件同时发生的可能性

例如：上午多云：0.4（不多云0.6），下午下雨：0.1（不下雨0.9）。

我们并不能确定早上的云是否与下午下雨的可能性有关。为此，需要查看两个变量所有可能结果的联合概率：
- 多云下雨：0.08
- 多云不下雨：0.32
- 不多云下雨：0.02
- 不对云不下雨：0.58

通过联合概率，我们可以推导出条件概率（利用条件概率的公式和某一个事件发生的概率）。如，考虑P(多云|下雨)
=P(多云, 下雨)/P(下雨)，通过联合概率P(多云, 下雨)和下雨的概率，可以得到P(多云|下雨)。

如果我们将下雨的概率视为一个常数，那么就可以改写公式为：P(多云,下雨)/P(下雨) = αP(多云,下雨)，或者
α<0.08, 0.02>。因此可以说：考虑到下午有雨，分解出α后就可以得到多云的可能性的比例。即：如果下雨，那么多云
和不多云的比是0.08:0.02。归一化后，即有P(多云 | 下雨) = <0.8, 0.2>

## 概率公式
- Negation否定律：P(¬a) = 1 - P(a)
- Inclusion-Exclusion包含-排除：P(a ∨ b) = P(a) + P(b) - P(a ∧ b)
- Marginalization边缘化：P(a) = P(a, b) + P(a, ¬b)，进一步的，有如下形式  
![边缘化](https://github.com/LBLBLBLB-XJTU/XJTU-CS-TryToRevive/blob/main/resource/marginalization.png)
- Conditioning调理：P(a) = P(a | b)P(b) + P(a | ¬b)P(¬b)，这只是将上一个公式多化了一步。同样的，有如下形式
![调理](https://github.com/LBLBLBLB-XJTU/XJTU-CS-TryToRevive/blob/main/resource/conditioning.png)

## 贝叶斯网络
贝叶斯网络是一种用于表示随机变量间的依赖关系的网络。它有以下性质：
- 是有向图
- 图中的每一个节点代表一个随机变量
- 从X到Y的箭头代表着X是Y的一个父节点，即Y的概率分布依赖于X的值
- 每一个节点X都有概率分布P(X | Parents(X))

值得注意得是，父子关系仅限于直接的父子关系。如A影响B，B影响C，但是不能说A会影响C。即P(A,B,C)=P(A)P(B|A)P(C|B)

## 推理
利用概率，可以得到新的信息，虽然得不到确认的信息，但是可以得到一些概率分布。推理有以下性质：
- 询问X：X是想要计算其概率分布的变量
- 证据变量E：针对事件e观察到的一个或多个变量
- 隐藏变量Y：没有被询问以及观察到的变量
- 目标：计算P(X | e).
### 枚举推理
枚举推理是通过e和一些Y得到X。  
![枚举推理](https://github.com/LBLBLBLB-XJTU/XJTU-CS-TryToRevive/blob/main/resource/inferencebyenumeration.png)  
在公式中，X代表着想要的变量，e是观察到的证据，y是所有的隐藏变量

## 采样
枚举推理需要大量的计算，因此可以损失一些精度来获取更快的速度。采样就是一种近似推理的方式。

在采样中，每一个变量被根据概率分布而取一个值。示例代码如下：
```angular2html
import pomegranate

from collections import Counter

from model import model

def generate_sample():

    # Mapping of random variable name to sample generated
    sample = {}

    # Mapping of distribution to sample generated
    parents = {}

    # Loop over all states, assuming topological order
    for state in model.states:

        # If we have a non-root node, sample conditional on parents
        if isinstance(state.distribution, pomegranate.ConditionalProbabilityTable):
            sample[state.name] = state.distribution.sample(parent_values=parents)

        # Otherwise, just sample from the distribution alone
        else:
            sample[state.name] = state.distribution.sample()

        # Keep track of the sampled value in the parents mapping
        parents[state.distribution] = sample[state.name]

    # Return generated sample
    return sample
```
### 似然权重
在采样时会丢弃一些东西，为了避免这样，可以采用似然权重：
- 先固定证据变量的值
- 使用贝叶斯网络中的条件变量采样隐藏变量
- 按可能性对每个样本进行加权

## 马尔可夫模型
像预测等任务，就需要考虑时间这一维度，为了表示这一个变量，我们引入一个新的变量，X，并改变这个变量。如
X<sub>t</sub>代表当前事件，而X<sub>t+1</sub>代表下一个事件。
### 马尔可夫假设
马尔可夫假设假设影响当前状态的只有有限的、固定数量的之前的状态
### 马尔可夫链
马尔可夫链是随机变量序列，其中每个变量的分布遵循马尔可夫假设。也就是说，链中的每个事件的发生都是基于其之
前事件的概率。

为了构建马尔可夫链，需要一个转移模型，模型将根据当前事件的可能值来获取下一个事件的概率分布。

## 隐式马尔可夫模型
隐式马尔可夫模型适用于具有隐藏状态的系统，这些系统会生成一些可以观察到的事件。人工智能有时可以对世界进行
一些测量，但无法了解世界的精确状态。在这些情况下，世界的状态称为隐藏状态，人工智能可以访问的任何数据都是
观察结果

例子：我们的人工智能想要推断天气（隐藏状态），但它只能访问室内摄像头，记录有多少人带了雨伞。一个传
感器模型Sensor Model（也称为发射模型Emission Model），可以表示。  
![传感器模型](https://github.com/LBLBLBLB-XJTU/XJTU-CS-TryToRevive/blob/main/resource/sensormodel.png)    
### 传感器马尔可夫假设
传感器马尔可夫假设假设证据变量仅取决于相应的状态

隐马尔可夫模型可以用两层马尔可夫链表示。顶层代表X<sub>t</sub>代表隐藏状态；底层代表E<sub>t</sub>代表证据即我们的观察

基于隐式，马尔可夫模型，可以完成多种任务：
- 过滤：基于从开始时的观察，计算现在状态的概率分布
- 预测：基于从开始时的观察，计算未来状态的概率分布
- 平滑：基于从开始时的观察，计算过去某个态的概率分布
- 最可能的解释：基于从开始时的观察，计算最可能的事件序列

下面是一个最可能解释的例子：
```angular2html
from pomegranate import *

# Observation model for each state
sun = DiscreteDistribution({
    "umbrella": 0.2,
    "no umbrella": 0.8
})

rain = DiscreteDistribution({
    "umbrella": 0.9,
    "no umbrella": 0.1
})

states = [sun, rain]

# Transition model
transitions = numpy.array(
    [[0.8, 0.2], # Tomorrow's predictions if today = sun
     [0.3, 0.7]] # Tomorrow's predictions if today = rain
)

# Starting probabilities
starts = numpy.array([0.5, 0.5])

# Create the model
model = HiddenMarkovModel.from_matrix(
    transitions, states, starts,
    state_names=["sun", "rain"]
)
model.bake()
```
注意到隐式马尔可夫模型中既需要传感器模型，也需要转移模型。

下面的代码片段有一串观察，由此可以得到最可能的解释。即基于观察的最可能的天气的模式
```angular2html
from model import model

# Observed data
observations = [
    "umbrella",
    "umbrella",
    "no umbrella",
    "umbrella",
    "umbrella",
    "umbrella",
    "umbrella",
    "no umbrella",
    "no umbrella"
]

# Predict underlying states
predictions = model.predict(observations)
for prediction in predictions:
    print(model.states[prediction].name)
```

