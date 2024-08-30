# 语言

- [语言](#--)
    * [语法与语义](#-----)
    * [上下文无关语法](#-------)
    * [nltk](#nltk)
    * [n元语法](#n---)
    * [标记化](#---)
    * [马尔可夫模型](#------)
    * [词袋模型](#----)
    * [朴素贝叶斯](#-----)
    * [词表示](#---)
    * [word2vec](#word2vec)
    * [神经网络](#----)
    * [注意力](#---)
    * [Transformer](#transformer)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

自然语言处理包含了所有以人类语言作为输入的任务,如自动总结、信息提取、语言识别、机器翻译、名称识别、语音转文字、文本识别、多意区分等

## 语法与语义

语法是句子的结构,语法可以是符合语法的也可以是模糊的.为了能够解析人类的语法,AI需要命令语法

语义是词或者句子的意思,句子可以完全符合语法,但是没有意义.为了解析人类的语言,AI需要命令语义

## 上下文无关语法

形式语法是是用语言生成句子的规则系统.对于上下文无关语法,文本被从其含义中抽象出来,以使用形式语法来表示句子的结构.有以下例子:

* She saw the city

这是一个简单的句子,我们可以生成一个语法树来表示他的结构

我们从为每个单词分配词性开始,She和city是名词,以N代表;Saw是动词,以V代表;The是限定词,来确定之后的名词是确定的还是不确定的,以D表示.这样,句子有以下表示:

* N V D N

这样,就可以将单词从语义意义抽象到词性.但是句子中的每个单词是和其他单词关联的,为了理解句子,必须知道词之间是如何连接的.

名词短语(NP)是连接到名词的一组单词,例如,She和city都是NP,she只有一个单词,而city前还有一个the.

动词短语(VP)是连接到动词的一组词,例如,saw是一个VP,而saw the cite也是一组VP,这时,一个VP由一个动词和一个NP组成,而NP又由一个限定词和一个名词组成.

最终,句子如下图

![语法树](https://github.com/LBLBLBLB-XJTU/XJTU-CS-TryToRevive/blob/main/resource/syntactictree.png)

使用形式语法,AI能够表示句子的结构.

## nltk

nlyk(natural language toolkit)是python中常用的语言处理的库.为了分析上面的句子,可以有如下的语法规则:

```
import nltk

grammar = nltk.CFG.fromstring("""
    S -> NP VP

    NP -> D N | N
    VP -> V | V NP

    D -> "the" | "a"
    N -> "she" | "city" | "car"
    V -> "saw" | "walked"
""")

parser = nltk.ChartParser(grammar)
```

在上面的代码中,箭头右侧为可能的单词,所以我们还添加了一些其他的可能的组件.

一个句子可以包含一个NP和一个VP,同时NP和VP自身也可以包含其他的组件.

```
sentence = input("Sentence: ").split()
try:
    for tree in parser.parse(sentence):
        tree.pretty_print()
        tree.draw()
except ValueError:
    print("No parse tree possible.")
```

将句子输入进去,函数就会打印出生成的语法树

## n元语法

n元语法是来自句子的样本的n个组件的序列.对于字符n元语法,组件是n个字符,对于单词n元语法,组件是单词.对于n,则表示在序列中有多少组件.

n元语法帮助AI处理文本.当AI在看见整个句子前,就已经看到了部分句子.一些单词往往更高频率的组合在一起,那么就可以预测下一个出现的单词.因此,在自然语言处理中,就可以把句子打成若干n元语法

## 标记化

标记化是将字符序列分片(tokens).tokens既可以是单词,也可以是句子,分别叫做单词标记化或者是句子标记化.我们需要先进行标记化才能使用n元语法,因为n元语法是依赖于标记序列的.

我们从使用空格来分割单词开始,但是则会导致得到带有符号的单词.因此要移除符号.但是我们会遇到一些带着撇号或者连字符的单词.而且一些符号如句号也很重要(当然我们也要区分如"Mr."的句号).处理这些问题的过程是标记化.最终完成标记化后,我们就可以开始n元语法

## 马尔可夫模型

马尔可夫模型可以用于生成文本.我们在文本上训练模型,根据其前面的n个单词建立n元语法中每个第n个标记的概率。例如:使用三元语法,在马尔可夫模型有两个单词后,可以基于这两个单词的概率分布来选择第三个单词

使用马尔可夫模型,我们能够生成语法和听起来与人类语言输出相似的文本.但是语句将缺少实际意义和目的

## 词袋模型

词袋模型是用词语的无序集合来表示文本的.这个模型忽略了语法,只考虑了词语的意思.这种方法可以用于分类任务,如情感分析.情感分析可以用于在购物评价中区分好评和差评.

## 朴素贝叶斯

朴素贝叶斯是可以用于词袋模型的情感分析.在情感分析中,我们会考虑基于当前词语句子是好评或是差评的概率是多少.即考虑P(sentiment|text).

例如P(positive|my grandson loves it),先对输入进行标记化:P(positive|my, garndson, loves, it),使用贝叶斯公式,有:$\frac{P(my, grandson, loves, it|positive)*P(positive)}{P(my, grandson ,loves it)}$.

我们可以通过不完全相等,而是近似值来简化句式子.由于概率的总和为1,可能将结果归一为精确的概率,即可以将式子简化为仅有分子上的部分$P(my, grandson, loves, it|positive)*P(positive)$.

接着,考虑到给定b的条件概率与a和b的联合概率成正比,则可以得到以下式子:$P(my, grandson, loves, it, positive)*P(positive)$.

到这里,计算联合概率仍然很复杂,因为需要计算之前的单词产生的概率分布.

这就是如何使用朴素贝叶斯:认为每个单词的概率独立于其他单词.这个不太正确但是能够产生很好的效果.使用这个假设,则式子为:$P(positive)*P(my|positive)*P(grandson|positive)*P(loves|positive)*P(it|positive)$.这个就不太难算了.P(positive)的概率即为样本中好评的概率,P(loved|Positive)等于包含“loved”一词的正样本数除以正样本数。

朴素贝叶斯的优势在于对于一种句子中出现概率高于另一个句子中概率的单词很敏感

但是一些单词可能不会在某种句子中出现,使概率为0.解决这个问题的方法是加法平滑,我们向分布中的每个值加一个$\alpha$来平滑数据.作为一种加法平滑,拉普拉斯平滑为每个值加1,假定每个值被观察过一次

## 词表示

one-hot表示是一种词表示的方法.每个单词都用一个向量表示，该向量由与单词一样多的值组成.如:

* [1,0,0,0]he
* [0,1,0,0]wrote
* [0,0,1,0]a
* [0,0,0,1]book

但是当单词数量上升,向量的长度也在增加.同时我们也不能重复表示一个单词.由此,有分布式表示,意义分布在向量的多个值中.通过分布式,可以限制向量的长度.如:

* [-0.34,-0.08,0.02,-0.12...]he
* [-0.27,0.40,0.00,-0.65,...]wrote
* [-0.12,-0.25,0.29,-0.09,...]a
* [-0.23,-0.16,-0.05,-0.57,...]book

这样可以使用较小的向量为每个单词生成独一的向量.

## word2vec

word2vec是生成分布式表示的算法之一.算法依赖于skip-gram结构,这是一个基于一个单词来预测文本的神经网络.神经网络为每个目标词都有一个输入单元,有一个小型的单层的隐藏层将生成表示单词的分布式表示的值,每个隐藏层单元会与每个输入层单元连接.输出层将生成可能出现在与目标单词相似的上下文中的单词.网络需要在数据集上使用bp算法训练

过程最后,每个单词将是一个向量或数字序列.

向量自身不表示什么意思.但是通过在语料库中找到具有最近似向量,我们可以通过一个函数来得到最相近的单词.我们也可以通过计算向量来得到单词间的差异有多大

通过使用神经网络和单词的分布式表示,我们让人工智能能够理解语言中单词之间的语义相似性,使我们离能够理解和生成人类语言的人工智能又近了一步

## 神经网络

一般来说,机器翻译使用了神经网络.在实践中,当我们翻译单词时,我们想要翻译一个句子或一个段落.这时,我们会遇到将固定长度句子翻译成不定长度句子的问题.

循环神经网络可以重复运行网络多次,跟踪保存相关信息.输入被带入网络,创建隐藏状态.将第二个输入与第一个隐藏状态一起传递到编码器中,会产生一个新的隐藏状态.重复此过程直到传递结束.接着解码器进行操作来获取单词直到结束.但是,在解码时,所有的信息都需要保存在一个状态中,需要大量的储存.因此合并隐藏层就称为了必要.同时,一些隐藏层比其他层重要,如何取舍也是问题

## 注意力

注意力机制可以帮助神经网络决定哪些值更加的重要.通过获取注意力分数,将它们乘上网络生成的隐藏状态的值,然后再相加,由此可以创建一个上下文向量,解码器利用该向量计算单词.在诸如此类的计算中出现的一个挑战是循环神经网络需要逐个单词的顺序训练,为此,并行训练是一种方法.

## Transformer

Transformer是一种新型的结构,单词同时进入到网络.输入单词进入神经网络并被捕获为编码表示.由于单词被同时输入到网络,很容易失去顺序,为此,位置编码被设置为输入.因此,网络会同时使用单词和单词的位置作为输入去编码表示.更进一步的,自注意力机制被加入去帮助输入的单词的上下文.事实上,网络会使用多层自注意力机制来更好的理解上下文.每个单词将多次进行,生成的结果可以更好的编码表示

在解码时,单词和单词的位置也进入到多个自注意力机制中.然后,多个注意力步骤被馈送到编码过程中的编码表示并提供给神经网络.由此,单词之间能够彼此之间注意.如果可以并行处理,那么计算将又快又好