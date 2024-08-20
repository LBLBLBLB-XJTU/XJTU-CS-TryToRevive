# 优化

- [优化](#--)
    * [局部搜索](#----)
    * [爬山算法](#----)
        + [局部最优和全局最优](#---------)
        + [爬山算法的变体](#-------)
    * [模拟退火算法](#------)
        + [旅行商问题](#-----)
    * [线性规划](#----)
    * [约束满足](#----)
    * [节点一致性](#-----)
    * [弧一致性](#----)
    * [回溯搜索](#----)
        + [推理](#--)
            - [最小剩余值](#-----)
            - [度启发](#---)
            - [最少约束](#----)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

## 局部搜索

局部搜索维持一个节点,通过向邻居借带你移动来实现搜索.局部搜索注重于寻找最佳的路径,当然往往不是最优的,而是足够好的(以节省算力).下面是一些术语

* 目标函数:这个函数用来最大化解决方案的价值
* 代价函数:这个函数用来最小化解决方案饿代价
* 当前状态:当前被函数考察的状态
* 相邻状态:当前状态可以转移到的状态

这个算法是在当前状态考虑一个节点,并移动它到他的邻居节点,而不是最大最小算法那种递归的考察

## 爬山算法

爬山算法是局部搜索的一种.算法中,邻居状态会与当前状态进行比较,如果有一个更好,那么就转移到那个状态.这个更好的定义取决于定义的目标函数.伪代码如下:

```
current = initial state of problem
repeat:
    neighbor = best valued neighbor of current
    if neighbor not better than current:
        return current
    current = neighbor
```

我们以一个当前状态开始(没有提供的话,则有随机产生).重复以下动作:评估并选择最好的邻居,用邻居的状态来替换当前状态.当没有邻居更好时,则结束.

这个算法有时无法或得最佳方案,陷入局部最优

### 局部最优和全局最优

局部最优是指某个状态相对于邻居,是最优的.而全局最优则是在全部的状态空间中是最优的

局部最优的一种情况是平坦最优,若干个相邻的等值排在一起形成一个平原,而平原相邻则是较差的状态.

### 爬山算法的变体

这些变体是为了解决局部最优问题的,但是仍然可能陷入局部最优.下面是希望寻找最大值的例子

* 最陡上升:选择值最高的邻居
* 随机:从值较高的邻居中随机选择,使得向任何能提高值的方向前进.例如如果价值最高的邻居导致局部最优,而立一个次高的邻居为全局最优,那么算法就有了意义
* 首选:选择第一个值比较高的邻居
* 随机重启:进行多次爬山,每次都从随机状态开始,比较每次的结果,选择最大的做为结果
* 局部束搜索:选择k个最高的邻居进行搜索

尽管局部搜索不能总是得到最优答案,但是能够提供一个较好的答案

## 模拟退火算法

模拟退火算法能够在陷入局部最优时,将算法拖出来

退火是指将金属加热,然后缓慢的冷却.这能够加强金属的韧性.类似的,模拟退火在开始时处于“较高的温度”,可能进行更多的随机选择,随机算法进行,“温度降低”,则减少随机选择,变得更加“坚韧”.算法允许改变当前的状态进入一个相对坏的状态,但是由此可以逃脱出局部最优状态.伪代码如下

```
current = initail state of problem
for t=1 to max:
    T = Temperature(t)
    neighbor = random neighbor of current
    ΔE = how much better neighbor is than current
    if ΔE > 0:
        current = neighbor
    with probability e^(ΔE/T) set current = neighbor
return current
```

算法以问题和一个Max作为输入,Max是要重复的次数.对于每次迭代,T由一个温度函数设置,这个函数在开始时返回较大的值,然后在迭代中逐步下降.接着,会随机选取一个邻居,通过$\Delta$E来量化好的程度.如果邻居更好,就切换到邻居.如果邻居没有优化,则用一个概率去考虑是否更换为邻居.这里的思想是,$\Delta$E越负,邻居被选择的概率越低;温度T越高,选择邻居的概率越高.也就是说,越开始,越好,邻居就越被选择;越靠后,越差,邻居被选择的概率就越低.数学上,$\Delta$E为负时,越小,则$e^{\frac{\Delta E}{T}}$越小,越接近0;而T越大,则$e^{\frac{\Delta E}{T}}$越大,越接近1

### 旅行商问题

旅行商问题是:用最短的距离来连接所有的点.即送货公司要找到一条最短的路,完成从公司到各个客户的送货

这个问题中,相邻状态可能被视为两个箭头交换位置的状态.模拟退火算法能够减少搜索的数量以提高速度.

## 线性规划

线性规划是优化线性方程的一系列问题(线性方程形如:$y=ax_1+bx_2+···$)

线性规划有如下组件:

* 追求最小化的代价函数:$c_1x_1+c_2x_2+···+c_nx_n$.每个x是变量,与一个c联系
* 一个约束,表明变量的和与一个阈值的<=或是严格的=关系:$a_nx_1+a_nx_2+...+a_nx_n \leq b$.$x_i$为变量,$a_i$是与变量有关的资源,b是用于解决这个问题的资源量
* 变量的边界:$l_i \leq x_i \leq u_i$.

下面是利用scipy的线性规划的代码例子:两台机器,一个50一小时需要5人,产出10单位,一个80一小时需要2人产出12单位.共有20人,需要90单位:

```
import scipy.optimize

# Objective Function: 50x_1 + 80x_2
# Constraint 1: 5x_1 + 2x_2 <= 20
# Constraint 2: -10x_1 + -12x_2 <= -90

result = scipy.optimize.linprog(
    [50, 80],  # Cost function: 50x_1 + 80x_2
    A_ub=[[5, 2], [-10, -12]],  # Coefficients for inequalities
    b_ub=[20, -90],  # Constraints for inequalities: 20 and -90
)

if result.success:
    print(f"X1: {round(result.x[0], 2)} hours")
    print(f"X2: {round(result.x[1], 2)} hours")
else:
    print("No solution")
```

## 约束满足

约束满足问题,是指需要给变量赋值以满足某些条件的问题

约束满足问题有如下组件:

* 一组变量:$(x_1,x_2,x_3...)$
* 一组每个变量对应的定义域:{$D_1,D_2,D_2...$}
* 一组限制:C

进一步术语有:

* 硬约束:对于解必须满足
* 软约束:表明一个方案好于另一个方案
* 一元约束:只关于一个变量的约束
* 二元约束:只关于两个变量的约束

## 节点一致性

节点一致性是指变量域中所有值都满足变量的一元约束

## 弧一致性

弧一致性是指变量域中所有值都满足变量的二元约束.换一句说,为了使X对于Y满足弧一致性,需要移除X的定义域中的元素直到X的每个可选值对应Y也有可选值

可以用下面的伪代码表示:(CSP代表约束满足问题)

```
function Revise(CSP,X,Y)
    revised = false
    for x in X.domain
        if no y in Y.domain satisfies constraint for (X,Y):
            delete x from X.domain
            revised = true
    return revised
```

算法以revised来标记X的定义域是否改变,然后检查么一个值,如果不满足就删除这个值

我们常常希望对于整个问题都是弧一致性的(而不仅仅是两个变量之间的),对此,有AC-3算法:

```
function(CSP)
    queue = all arcs in CSP 
    while queue not empty:
        (X,Y)=Dequeue(queue)
        if Revise(CSP,X,Y):
            if size of X.domain == 0:
                return false
            for each Z in X.nieghbors - {Y}
                Enqueue(queue,(Z,X))
    return true

```

算法将所有的弧加入到队列中,每次考虑一个弧,然后移除这个弧.考虑时,使用Revise来看是否满足弧一致性.如果发生了改变,则要检查改变后的定义域是否为空(即是否有解),如果改变后有解,就要考察改变后的所有弧是否满足(即加入除Y以外的弧).如果没有改变,则继续算法

注意,弧一致性能够简化问题,但是不一定能解决问题,因为只考虑了二进制约束,而不考虑多个节点之间的连接

## 回溯搜索

回溯搜索是一种搜索算法,它考虑了约束满足搜索问题结构.简而言之,是一个递归的算法,只要当前赋值满足约束就继续赋值,如果不满足,就改变之前赋值.伪代码如下:

```
function Backtrack(assignment, CSP):
    if assignment complete:
        return assignemnt
    var = Select-Unassigned-Var(assignemnt, CSP)
    for value in Domain-Value(var, assignemnt,CSP):
        if value consistent with assignment:
            add {var = value} to assignment
            result = Backtrack(assignment,CSP)
            if result != failure:
                return result
            remove{var = value} from assigment
    return failure
```

算法首先检查是否满足条件;如果算法未完成,算法会选择一个未赋值的变量,然后对这个变量尝试着赋值,然后递归的继续回溯算法;然后会检查结果,如果成功则返回;若不成功,则最新的赋值则会被移除,然后尝试新的赋值.如果说定义域中的所有的值都返回failure,则说明需要回溯(上个赋值就失败了).如果回到了最开始的变量,则说明失败

### 推理

虽然回溯搜索已然加强了不少,但是仍然不够好.通过强制弧一致性联合在一起,我们能够得到一个更有效率的算法,这个算法称为弧一致性维持算法.这个算法会在每次赋值时维护弧一致性.具体的,在每一次赋值后,会调用AC-3算法,并以一个所有弧(Y,X)组成的队列开始,其中的Y是X的邻居.接下来的是修改后的回溯,保持了弧的一致

```
function Backtrack(assignment, csp):

    if assignment complete:
        return assignment
    var = Select-Unassigned-Var(assignment, csp)
    for value in Domain-Values(var, assignment, csp):
         if value consistent with assignment:
             add {var = value} to assignment
             inferences = Inference(assignment, csp)
             if inferences ≠ failure:
                 add inferences to assignment
             result = Backtrack(assignment, csp)
             if result ≠ failure:
                 return result
             remove {var = value} and inferences from assignment
    return failure
```

其中的推理函数,使用了AC-3算法,输出的结果是所有的可以通过弧一致性推理出的推理结果.

至此,所有的算法中的选择是随机的,通过启发式函数可以优化这一个过程.

#### 最小剩余值

MRV就是这样的一种启发式函数,其中的思想是:如果变量的定义域被推理所限制,限制到只有一个值(或者两个值),那么通过这个赋值,我们可以减少所需的回溯的次数.即我们早晚需要作出这个赋值,如果这个赋值带来错误,则尽快的查清并不再回溯

#### 度启发

度启发依赖于每个变量的度,即有多少变量连接到这个变量.通过赋值度最高的变量,我们可以限制多个变量

#### 最少约束

最少约束算法,会选择赋值后对其他变量约束最少的算法.这个算法与度启发相反,希望赋值后会限制最少的变量.其中的思想是,找到最麻烦的变量,然后尽可能使其麻烦最小
