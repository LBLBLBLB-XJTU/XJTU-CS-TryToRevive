# 搜索问题

- [搜索问题](#----)
  * [概述](#--)
  * [概念](#--)
  * [如何解决搜索问题](#--------)
    + [解决方法](#----)
    + [数据结构](#----)
  * [常用搜索算法](#------)
    + [DFS深度优先搜索](#dfs------)
    + [BFS广度优先搜索](#bfs------)
    + [贪婪搜索](#----)
    + [A*算法](#a---)
  * [对抗性搜索](#-----)
    + [Minimax算法](#minimax--)
    + [Alpha-Beta Pruning横向剪枝算法](#alpha-beta-pruning------)
    + [深度限制的MiniMax算法](#-----minimax--)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

## 概述
搜索问题主要涉及：基于一个初始状态和目标状态，返回一个从初始到目标的方法。例如：导航软件在基于你处在的位置（初始）和你的目的地（目标），给出一个到达的路径（方法）

## 概念
1. Agent，智能体：一个实体，可以感知环境并基于环境进行操作。在上面的导航的例子中，一辆汽车可以作为一个agent。
2. State，状态：agent处在的环境，在上面的导航例子中，距终点100米的位置是一个状态，而距离50米则是另一个状态。
3. Actions，行为：实体在某个状态下可以进行行为，如向东走10米，或者向西走十米。在搜索问题中，actions被定义为一个函数，以状态s为输入，Actions(s)会返回一个在状态s可以执行的行为的集合。
4. Transition Model，转移模型：对于在一个状态下执行操作后转移到另一个状态的描述。Transition Model也被定义为一个函数，以状态s和行为a作为输入，Results(s,a)会返回一个新的状态，这个状态是在状态s下执行a产生的
5. State Space，状态空间：从初始状态，可以抵达的所有的状态的集合
6. Goal Test，目的测试：检测是否到达了目的
7. Path Cost，代价：从初始状态抵达目标状态花费，依据此可以确定最好的方法。在导航的例子中，路程或时间可以
当作代价

## 如何解决搜索问题
### 解决方法
解决方法以可行的行动序列来表示。最佳的解决方法(Optimal Solution)则是代价最小的行动序列。
### 数据结构
对于搜索问题，数据被存储在节点中，这个数据结构应有以下四部分：
- 状态，当前所处的状态
- 父节点，本节点由父节点转移生成
- 行为，父节点产生本节点进行的行为
- 代价，从初始状态到本节点的状态所需的代价   

状态可以检测是否达到了目标；父节点和行为则可以通过回溯生成行为序列，即方法；而代价则可以通过比较来寻找最优方法

而对于具体的搜索过程，我们则需要使用一种机制frontier（边界）来实现搜索。这个机制从包含初始状态和一个空的搜索过的节点的集合，通过重复一组操作来到达目标。伪代码如下：
```angular2html
1. 如果frontier（待搜索的集合）为空，则说明搜索失败，无解。开始时，frontier中将只包含初始状态。
2. 从frontier中移除一个节点，并考虑这个节点
3. 如果这个节点是目标，则停止。否则，扩展这个节点（找出所有的能够通过这个节点抵达的新节点），并把这些新节点添加到frontier中。然后把这个节点加入到搜索过的节点的集合中
```
进一步的，如果两个节点之间是双向的，那么上面的伪代码则可能进入无限的循环中。为此我们进行改进：
```angular2html
1. 如果frontier（待搜索的集合）为空，则说明搜索失败，无解。开始时，frontier中将只包含初始状态。
2. 从frontier中移除一个节点，并考虑这个节点
3. 如果这个节点是目标，则停止。扩展这个节点（找出所有的能够通过这个节点抵达的新节点），并把这些新节点添加到frontier中。然后把这个节点加入到搜索过的节点的集合中
4. 将这个节点加入到explored set。
5. 如果这个节点既不在frontier中，也不在explored set中。扩展这个节点（找出所有的能够通过这个节点抵达的新节点），并把这些新节点添加到frontier中。然后把这个节点加入到搜索过的节点的集合中 
```

## 常用搜索算法
### DFS深度优先搜索
在上面关于frontier的伪代码中，在第二步，我们并没有确定移除哪一个节点。这个选择会影响搜索的过程以及速度。我们主要介绍两种选择方法，即用栈存储节点的DFS和用队列存储节点的BFS。
- 简述   
DFS的基本思想是在一个方向上测试到底，然后在换一个方向。一般会使用一个栈来存储节点，FILO的条件说明了如何选择节点：在frontier中，这个被选择的节点应该是最后一个加入到frontier的节点（最新的）。这会让算法在一个方向上尽可能深的探索 ，探索完一个方向后，然后在切换到另一个方向。
- 优点  
  - 最佳情况下，这将是最快的算法，将花费最少的时间
- 缺点
  - 找到的方法可能不是最优的方法
  - 最坏情况下，会搜索每一种情况花费最长的时间来找到方法
- 代码示例：
```angular2html
# Define the function that removes a node from the frontier and returns it.
def remove(self):
    # Terminate the search if the frontier is empty, because this means that there is no solution.
    if self.empty():
        raise Exception("empty frontier")
    else:
            # Save the last item in the list (which is the newest node added)
        node = self.frontier[-1]
        # Save all the items on the list besides the last node (i.e. removing the last node)
        self.frontier = self.frontier[:-1]
        return node
```
### BFS广度优先搜索
BFS与DFS正好相反，则使用队列来实现
- 简述
BFS的基本思想是搜索所有的方向，在每一个方向上进行一次搜索。使用队列来存储节点，FIFO的条件说明了如如何选择节点：在frontier中，这个被选择的节点是最开始被加入的节点（最老的）。这回让算法一层一层的搜索。
- 优点
  - 能够保证方法是最优的
- 缺点
  - 该算法几乎可以确认运行时间比最短时间长
  - 最坏情况下，算法会消耗最长的时间
- 代码示例:
```angular2html
# Define the function that removes a node from the frontier and returns it.
def remove(self):
    # Terminate the search if the frontier is empty, because this means that there is no solution.
    if self.empty():
        raise Exception("empty frontier")
    else:
        # Save the oldest item on the list (which was the first one to be added)
        node = self.frontier[0]
        # Save all the items on the list besides the first one (i.e. removing the first node)
        self.frontier = self.frontier[1:]
        return node
```
### 贪婪搜索
BFS和DFS在搜索过程中，都没有利用到过程中所获取的知识，而往往这些知识能够优化搜索的过程。
- 简述  
贪婪搜索会扩展离目标最近的节点，这个最近的度量，则由启发式函数h(n)来决定，（然而这个最近的度量很有可能是错误的）贪婪算法的效率取决于h(n)的好坏。例如，在迷宫中，距离终点的曼哈顿距离（直线距离）就是一种h(n)，毫无疑问，曼哈顿距离没有考虑到迷宫中复杂的墙体。因此，与知识有关的算法有可能比与知识无关的算法慢
### A*算法
A*算法是贪婪算法的一种改进
- 简述
A*算法不仅考虑到h(n),还考虑了到达当前位置的花费g(n)。通过结合这两个值，算法可以更正确的选择路径。算法会记录预估到达目标的代价和从初始到目前的代价，当这个和超过之前的某个选择时，算法就会放弃当前路径而回退，以防止自己沿着由于h(n)的错误标记而低效的路径继续下去。但是，算法的优劣仍与h(n)息息相关，以至于在某些情况下比贪婪算法还要差
- h(n)要求：
  - 不能高估真实成本
  - 一致性（自增性），从本节点到新节点的代价加上新节点到目标的估计要大于等于本节点到目标的估计。即：对于节点n和后继节点n\`,之间的代价为c，则有h(n)&le;h(n\`)+c 

## 对抗性搜索
在对抗性搜索中，算法将面对一个对手，这个对手试图实现与算法完全相反的目标。井字棋即是一种代表性的对抗性的竞技
### Minimax算法
- 简述  
Minimax算法有如下特征：一方的胜利条件为-1而另一方胜利的条件为1，双方为了各自的目标进行行动，-1的一方希望分数尽可能少，而1的一方则希望分数尽可能多。
- 问题中的AI构成
  - S<sub>0</sub>：初始状态。在井字棋中，为3乘3的棋盘
  - Player(s)：一个函数，输入状态，返回该哪一个棋手下棋
  - Actions(s)：一个函数，输入状态，返回所有在此状态下的合法移动，在井字棋中，为看哪一个位置是空的
  - Result(s,a)：一个函数，输入状态s和行为a，返回新的状态。在井字棋中，即落子后新的棋盘状态
  - Terminal(s)：一个函数，输入状态，判断是否有人获胜或是二人平局（即游戏结束）
  - Utility(s)：一个函数，输入一个最终状态，返回状态的值（-1，0，1）
- 算法
算法会递归的模拟所有可能的情况，直到最终状态。算法依据当前的情况（谁的回合，怎么下最优），选择行为。这样来，
AI会在寻求最大和最小之间进行转换，并为每个可能的行为创建值。  
具体的：求值最大的棋手会询问：“我行动后，会产生一个新状态；如果求最小值的棋手进行最优的行为，那么他会怎么操作来获得最小值呢？”。为了回答这个问题，求最大值的棋手不得不问：“为了知道最小值棋手怎么行动（最优的行动），我需要模拟求最小值的棋手来询问同样的问题：‘如果我采取行动，最大值的棋手会会怎么才做来获得最大值呢？’”。这就形成了一个难以理解的递归。通过这个递归推理，求最大值的棋手会为每一个由可能的行为生成的可能的状态创建一个值，由此进行行动
- 伪代码
```angular2html
给出状态s:
    1. 求最大值的棋手在Actions(s)中挑选产生Min-Value(Result(s,a))的行为a
    2. 求最小值的棋手在Actions(s)中挑选产生Max-Value(Result(s,a))的行为a
函数Max-Value(state)
    1. v = -&infin;
    2. if Terminal(state):return Utility(state)
    3. for action in Actions(state):
        v = Max(v, Min-Value(Result(state, action)))
       return v
函数Min-Value(state)
    1. v = &infin;
    2. if Terminal(state):return Utility(state)
    3. for action in Action(state):
        v = Min(v, Max-Value(Result(state, action)))
       return v
```
### Alpha-Beta Pruning横向剪枝算法
- 简述
横向剪枝算法去除了一些递归计算的过程以实现对MiniMax算法的优化。他的基本思想是：如果有证据表明，采取一个
行动会让对手获利（使对手获得比当前还要好的分数），那么就不必考虑这一明显不利的行为  
更具体的：最大化的棋手知道，在自己执行一步后，对手将设法取得最低的分数。假定最大化玩家有三种可能的行为：
其中第一种行为的价值为4。棋手会为下一个动作生成价值。棋手能够通过计算生成对手进行最小化的行为，并知道对手会
选择其中会使分数最低的。然而，在完成计算生成对手最小化行为之前，最大化棋手看到晴哦中一个选项的价值为3。这意味着最小话化玩家没有理由去探索其他的动作（因为其他动作不会比三小）。这样一来，尚未计算的值便不重要了：
  - 如果为10，那么最小化玩家肯定不会选择
  - 如果为-10，那么这比3还小，对最大化的玩家更加不利  
因此这些计算可去除，因为最大化玩家已有更好的选择
![横向剪枝](https://github.com/LBLBLBLB-XJTU/XJTU-CS-TryToRevive/blob/main/resource/alphabeta.png?raw=true)
- 算法实现
```angular2html
# 递归实现Alpha-Beta剪枝
def alpha_beta_search(state):
    def max_value(state, alpha, beta):
        if terminal(state):
            return utility(state)
        v = float('-inf')
        for action in actions(state):
            v = max(v, min_value(result(state, action), alpha, beta))
            if v >= beta:
                return v  # 剪枝
            alpha = max(alpha, v)
        return v
   
    def min_value(state, alpha, beta):
        if terminal(state):
            return utility(state)
        v = float('inf')
        for action in actions(state):
            v = min(v, max_value(result(state, action), alpha, beta))
            if v <= alpha:
                return v  # 剪枝
            beta = min(beta, v)
        return v
    
    best_score = float('-inf')
    beta = float('inf')
    best_action = None
    for action in actions(state):
        v = min_value(result(state, action), best_score, beta)
        if v > best_score:
            best_score = v
            best_action = action
    return best_action
```
### 深度限制的MiniMax算法
- 简述
由于MiniMax会穷尽所有的情况，是不可计算的。我们需要对步数进行限制。然而这就无法为行为获得一个精确的值（由于
比赛没有结束）。为此，这个算法使用一个评估函数来解决这个问题。这个函数会从状态来计算预期的值（分配一个值）。
例如，在国际象棋中，输入棋盘状态，尝试评估预期的值，然后返回一个正的或者负的值（表示有利程度）。利用这些值来
决定行动。因此，评估函数越好，算法就越好
- 代码示例
```angular2html
def depth_limited_minimax(state, depth, is_maximizing_player):
    
    if depth == 0 or terminal(state):  # 终止条件：达到最大深度或终局状态
        return utility(state)
    
    if is_maximizing_player:
        max_eval = float('-inf')
        for action in actions(state):
            eval = depth_limited_minimax(result(state, action), depth - 1, False)
            max_eval = max(max_eval, eval)
        return max_eval
    else:
        min_eval = float('inf')
        for action in actions(state):
            eval = depth_limited_minimax(result(state, action), depth - 1, True)
            min_eval = min(min_eval, eval)
        return min_eval
```