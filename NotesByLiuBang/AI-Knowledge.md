# Knowledge知识

- [Knowledge知识](#knowledge--)
  * [Sentence](#sentence)
  * [命题逻辑](#----)
    + [命题符号](#----)
    + [命题连词](#----)
    + [知识模型、知识基](#--------)
    + [蕴含](#--)
  * [推理](#--)
    + [模型检查算法](#------)
  * [知识工程](#----)
  * [推理规则](#----)
    + [Modus Ponens肯定前件](#modus-ponens----)
    + [And Elimination与消去](#and-elimination---)
    + [Double Negation Elimination双重否定](#double-negation-elimination----)
    + [Implication Elimination推理消去](#implication-elimination----)
    + [Biconditional Elimination双条件消去](#biconditional-elimination-----)
    + [De Morgan's law德摩根律](#de-morgan-s-law----)
    + [Distributive Property分配律](#distributive-property---)
    + [知识与搜索问题](#-------)
  * [消解](#--)
    + [消解](#---1)
    + [子句与CNF](#---cnf)
  * [一阶逻辑](#----)
    + [全称量化](#----)
    + [存在量化](#----)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

## Sentence
AI通过Sentence来存储和利用知识。sentence是在知识表示语言中关于一个内容的断言

## 命题逻辑
命题逻辑基于命题，可真也可假
### 命题符号
P，Q，R这三个字母常用于作为命题符号
### 命题连词
- Not(¬)：对命题取反  
- And(∧)：只有连接的两个命题都为真时，才取真
- Or(∨)：只要有一个为真即取真（这里有一个不常用的排他性Or（即XOR(⊕)），排他性的Or只有两个中一真一假才为真，这个不常用）
- Implication(→)：如果前者成立，那么后者成立。称前者为因，后者为果。当原因为假，无论结果如何，整句始终为真，当原因为真，那么整句的真假与结果的真假一致
- 相等(↔)：即双向的推理。此时，全假和全真使全句为真，一真一假则为假
### 知识模型、知识基
知识由命题组成。知识基是提供给AI的一组sentence的集合，AI由此作出推断
### 蕴含
⊨符号如果A蕴含B，那么如果A为真，则B就真。注意，蕴含与推断不同，推理是两个命题之间的逻辑连接词，另一方面，蕴涵是一种关系，意味着如果 α 中的所有信息都是真的，那么 β 中的所有信息都是真的。

## 推理
推理，即是从已有的知识中推理出新的知识。这里有许多种方式来推断出新的知识。我们从模型检查（Model Check）算法开始
### 模型检查算法
为了证明if KB ⊨ a（即我们可以从已有的知识中退出a），我们需要：  
- 枚举所有可能的模型
- 如果在每一个KB为真的模型中，a也为真，那么则KB蕴含a  

这个过程可由如下的代码表示。首先，给出的是知识与逻辑是怎么保存的
```angular2html
from logic import *

# Create new classes, each having a name, or a symbol, representing each proposition.
rain = Symbol("rain")  # It is raining.
hagrid = Symbol("hagrid")  # Harry visited Hagrid
dumbledore = Symbol("dumbledore")  # Harry visited Dumbledore

# Save sentences into the KB
knowledge = And(  # Starting from the "And" logical connective, becasue each proposition represents knowledge that we know to be true.

    Implication(Not(rain), hagrid),  # ¬(It is raining) → (Harry visited Hagrid)

    Or(hagrid, dumbledore),  # (Harry visited Hagrid) ∨ (Harry visited Dumbledore).

    Not(And(hagrid, dumbledore)),  # ¬(Harry visited Hagrid ∧ Harry visited Dumbledore) i.e. Harry did not visit both Hagrid and Dumbledore.

    dumbledore  # Harry visited Dumbledore. Note that while previous propositions contained multiple symbols with connectors, this is a proposition consisting of one symbol. This means that we take as a fact that, in this KB, Harry visited Dumbledore.
    )
```
为了实现模型检查算法，我们需要以下信息：
- KB，用来得到结论
- 问句，即是我们想要推理出的结论
- 一套用于表示的符号
- 模型，将真值和假值分撇给符号

那么，模型检查算法如下所示：
```angular2html
def check_all(knowledge, query, symbols, model):

    # If model has an assignment for each symbol
    # (The logic below might be a little confusing: we start with a list of symbols. The function is recursive, and every time it calls itself it pops one symbol from the symbols list and generates models from it. Thus, when the symbols list is empty, we know that we finished generating models with every possible truth assignment of symbols.)
    if not symbols:

        # If knowledge base is true in model, then query must also be true
        if knowledge.evaluate(model):
            return query.evaluate(model)
        return True
    else:

        # Choose one of the remaining unused symbols
        remaining = symbols.copy()
        p = remaining.pop()

        # Create a model where the symbol is true
        model_true = model.copy()
        model_true[p] = True

        # Create a model where the symbol is false
        model_false = model.copy()
        model_false[p] = False

        # Ensure entailment holds in both models
        return(check_all(knowledge, query, remaining, model_true) and check_all(knowledge, query, remaining, model_false))
```
这是一个递归的算法：每次都会从符号集中弹出一个符号，并通过这个符号生成两个模型，这两个模型一个是符号取值为真
一个符号取值为假，然后递归调用函数，这样一来，所有的符号都会被赋予一个值。当符号集为空时，即可认为生成了所有可能的模型。这样递归到最后一层时即可检查在模型中，KB是否为真；如果KB为真，那么就检查结论是否为真。

注意，我们只对当KB为真时的模型感兴趣，如果KB为假，那么模型中发生的事情与我们无关

## 知识工程
知识工程时弄清楚如何在AI中表示命题和逻辑的过程

## 推理规则
模型检查算法由于要检查所有的可能的模型，因此会在效率上有问题。而推理规则帮助我们推理而不必检查所有的可能性

推理规则常常会用一个水平线表示，线上方的是前提，即拥有的知识。而下方则是结论，即从前提可以生成的知识

### Modus Ponens肯定前件
如果一个蕴涵及其前件为真，那么结果也为真。  
![肯定前件](https://github.com/LBLBLBLB-XJTU/XJTU-CS-TryToRevive/blob/main/resource/sm%20.png)
### And Elimination与消去
如果一个与命题为真，那么其中任何的原子命题为真。
![与消去](https://github.com/LBLBLBLB-XJTU/XJTU-CS-TryToRevive/blob/main/resource/andelimination.png?raw=true)
### Double Negation Elimination双重否定
一个命题被否定两次时为真  
![双重否定](https://github.com/LBLBLBLB-XJTU/XJTU-CS-TryToRevive/blob/main/resource/doublenegationelimination.png?raw=true)
### Implication Elimination推理消去
这和肯定前件相似但又有不同。一个推断等价于前提取反然后或上结论
![推理消去](https://github.com/LBLBLBLB-XJTU/XJTU-CS-TryToRevive/blob/main/resource/implicationelimination.png?raw=true)
### Biconditional Elimination双条件消去
一个双条件命题等价于推断和反向推断的与。
![双条件消去](https://github.com/LBLBLBLB-XJTU/XJTU-CS-TryToRevive/blob/main/resource/biconditionalelimination.png?raw=true)
### De Morgan's law德摩根律
为了使And命题为真，Or命题中至少有一个命题必须为真。同样的，为了使Or命题为真，and命题中至少一个为真
这个定律用于and和or的转换  
![德摩根律1](https://github.com/LBLBLBLB-XJTU/XJTU-CS-TryToRevive/blob/main/resource/demorgans1.png?raw=true)
![德摩根律2](https://github.com/LBLBLBLB-XJTU/XJTU-CS-TryToRevive/blob/main/resource/demorgans2.png?raw=true)
### Distributive Property分配律
具有用 And 或 Or 连接词分组的两个元素的命题可以分布或分解为由 And 和 Or 组成的更小的单元
![分配律1](https://github.com/LBLBLBLB-XJTU/XJTU-CS-TryToRevive/blob/main/resource/distributive1.png?raw=true)
![分配律2](https://github.com/LBLBLBLB-XJTU/XJTU-CS-TryToRevive/blob/main/resource/distributive2.png?raw=true)
### 知识与搜索问题
推理可以被视为一种搜索问题：
- 初始状态：开始时的KB
- 行为：推理规则
- 转移模型：推理后形成的新的KB
- 目标测试：检查论述是不是我想要证明的
- 代价计算：用了多少步来推理

## 消解
### 消解 
消解是一个强大的推理规则：对于一个与命题，如果其中一个为假，则另一个必须为真
![消解1](https://github.com/LBLBLBLB-XJTU/XJTU-CS-TryToRevive/blob/main/resource/resolution1.png?raw=true)

消解依赖于互补文字（Complementary Literals，两个相同的原子命题，一个为否定一个不否定。互补文字帮助我们生成新的语句来进行推理

进一步的，消解有如下的扩展：
![消解2](https://github.com/LBLBLBLB-XJTU/XJTU-CS-TryToRevive/blob/main/resource/resolution2.png?raw=true)

### 子句与CNF
子句是文字的析取，析取是命题通过Or形成的。另一方面，合取是命题通过And形成的。子句可以让我们将任何语句转换为
CNF（合取范式），即子句的合取

为了将命题转换为CNF，需以下步骤：
- 消除等价：Turn (α ↔ β) into (α → β) ∧ (β → α)
- 消除推理：Turn (α → β) into ¬α ∨ β
- 使用德摩根律，将否定向内移动直至只有文字被否定：Turn ¬(α ∧ β) into ¬α ∨ ¬β

转换到CNF后，会有子句中有相同的文字，这时可以进行分解，相同的文字只需保留一个

消解文字及其否定，可以得倒空子句，空子句总是假的。由此可以推出消解算法：
- 为了确定KB ⊨ α是否为真：
  - 检查KB ∧ ¬α是否矛盾：
    - 矛盾，则真
    - 否则为假

这个矛盾算法常常被CS学科使用，因此，进一步的：
- 为了确定KB ⊨ α是否为真：
  - 将KB ∧ ¬α转换为CNF
  - 持续进行消解：
    - 如果产生了空子句，则证明为真
    - 否则则为假

## 一阶逻辑
一阶逻辑比命题逻辑更为简洁的来表示复杂的信息。一阶逻辑使用常量符号和谓词符号。常量符号表示实物，而谓词符号
则类似于关系或者函数，带有参数并返回真或假
### 全称量化
全称量化利用∀ 来表示“对于所有”
### 存在量化
存在量化利用∃ 来表示至少一个。


