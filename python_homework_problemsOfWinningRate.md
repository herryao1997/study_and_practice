# 题目一（函数的定义及调用）


```python
def func(x, *y, **z):
    print(x)
    print(y)
    print(z)
```


```python
ls = ['a', 'b', 'c']
```


```python
print(ls) 
```

    ['a', 'b', 'c']



```python
d = {"name": "Sarah", "age":18}
```


```python
func(*ls, 'b', **d)
```

    a
    ('b', 'c', 'b')
    {'name': 'Sarah', 'age': 18}


# 题目二 （函数式编程）


```python
import random


def get_input():
    #输入原始数据
    prob_A = eval(input("请输入运动员A每球获胜的概率（0-1）>")) 
    prob_B = 1 - prob_A
    number_of_games = eval(input("请输入比赛的总场数（正数）>"))
    print("A 选手获胜的概率为：", prob_A)
    print("B 选手获胜的概率为：", prob_B)
    print("模拟总次数为：", number_of_games)
    return prob_A, prob_B, number_of_games


def game_over(score_A, score_B):
    #如果有任意一方得分达到21比赛结束
    return score_A ==21 or score_B == 21


def sim_one_game(prob_A, prob_B):
    score_A , score_B = 0, 0
    while not game_over(score_A, score_B):
        if random.random() < prob_A:
            score_A += 1
        else:
            score_B += 1
    return score_A, score_B

def sim_n_game(prob_A, prob_B, number_of_games):
    win_A, win_B = 0, 0                    #初始化模拟一次比赛的比分
    for i in range(number_of_games):       #循环number_of_games次
        score_A, score_B = sim_one_game(prob_A, prob_B)       #获取当前模拟的比赛结果
        if score_A > score_B:              
            win_A += 1
        else:
            win_B += 1
    return win_A, win_B


def print_summary(win_A, win_B, number_of_games):
    print("总共模拟了{}次比赛".format(number_of_games))
    print("选手A获胜{0}场，或胜率为{1:.1%}".format(win_A, win_A/number_of_games))
    print("选手B获胜{0}场，或胜率为{1:.1%}".format(win_B, win_B/number_of_games))


def main():
    #主要逻辑
    prob_A, prob_B, number_of_games = get_input()       #获取数据
    win_A, win_B = sim_n_game(prob_A, prob_B, number_of_games)            #获取模拟结果
    print_summary(win_A, win_B, number_of_games)        #汇总模拟结果

    
if __name__ == "__main__":
    main()    
        
    
```

    请输入运动员A每球获胜的概率（0-1）>0.3
    请输入比赛的总场数（正数）>10000
    A 选手获胜的概率为： 0.3
    B 选手获胜的概率为： 0.7
    模拟总次数为： 10000
    总共模拟了10000次比赛
    选手A获胜42场，或胜率为0.4%
    选手B获胜9958场，或胜率为99.6%


* 对get_inputs函数增加容错，当输入的单球获胜概率不在(0,1)区间内时，提示输入错误，重新输入。

* 将每局比赛的获胜规则更改为：一方得分大于等于21分，且与对手分差至少为2分，该种情形第一次出现时，得分高者获得该局比赛的胜利。

* 将每场比赛获胜规则更改为：三局两胜，率先获得两局胜利者，获得该场比赛胜利。

* 其他条件于本章例题相同，试模拟1万场比赛的结果。


```python
import random


def get_input():
#    prob_A = eval(input("please input the probability for A（0-1）>"))
#    while prob_A > 1 or prob_A < 0:
#        prob_A = eval(input("please renter a num"))
    while True:
        prob_A = eval(input("please input the probability for A（0-1）>"))
        if prob_A >0 and prob_A <1:
            break
    prob_B = 1 - prob_A
    num_of_games = eval( input("please input gamenums （positive value）>"))
    print("the probability for A is", prob_A)
    print("the probability for B is", prob_B)
    print("the total num for simulation is", num_of_games)
    return prob_A, prob_B, num_of_games


def game_over(score_A, score_B):
    return (score_A >= 21 or score_B >= 21) and (abs(score_A - score_B) >= 2)
#经过断点调试可知，如下的代码由于当第一条要求满足时，第二条要求不满足，继续累加会造成永远无法满足，导致最终的死循环～
#    return (score_A == 21 or score_B == 21) and (abs(score_A - score_B) >= 2)

def sim_one_games(prob_A, prob_B):
    cnt_A, cnt_B = 0, 0      #初始化胜利次数
    for i in range(5):     #循环最多五次   
        score_A, score_B = 0, 0
        while not game_over(score_A, score_B):
            if random.random() < prob_A:
                score_A += 1
            else:
                score_B += 1 #一次循环结束得到比分
        if score_A > score_B: #判断一次比分的大小决定胜负
            cnt_A +=1
        else:
            cnt_B +=1        #一次比赛结束，结束记录
        if cnt_A == 3 or cnt_B == 3: #如果有一个人的胜利次数先达到3则比赛结束退出循环
            break
    return cnt_A, cnt_B
            
    
def sim_n_games(prob_A, prob_B, num_of_games):
    win_A, win_B = 0, 0
    for i in range(num_of_games):
        cnt_A, cnt_B = sim_one_games(prob_A, prob_B)
        if cnt_A > cnt_B:
            win_A += 1
        else:
            win_B += 1
    return win_A, win_B


def print_summary(win_A, win_B, num_of_games):
    print("总共模拟的次数为{0}".format(num_of_games))
    print("A总共胜利了{0}次，其胜率为{1:.1%}".format(win_A, win_A/num_of_games))
    print("B总共胜利了{0}次，其胜率为{1:.1%}".format(win_B, win_B/num_of_games))

def main():
    prob_A, prob_B, num_of_games = get_input()
    win_A, win_B = sim_n_games(prob_A, prob_B, num_of_games)
    print_summary(win_A, win_B, num_of_games)

    
if __name__ == "__main__":
    main()
    
    
```

    please input the probability for A（0-1）>0.48
    please input gamenums （positive value）>100000
    the probability for A is 0.48
    the probability for B is 0.52
    the total num for simulation is 100000
    总共模拟的次数为100000
    A总共胜利了31264次，其胜率为31.3%
    B总共胜利了68736次，其胜率为68.7%


# 题目三（匿名函数lambda）


```python
ls = "how old are you??"
d = {}
for i in ls:
    d[i] = d.get(i, 0) + 1
    print(d)
print(d)
```

    {'h': 1}
    {'h': 1, 'o': 1}
    {'h': 1, 'o': 1, 'w': 1}
    {'h': 1, 'o': 1, 'w': 1, ' ': 1}
    {'h': 1, 'o': 2, 'w': 1, ' ': 1}
    {'h': 1, 'o': 2, 'w': 1, ' ': 1, 'l': 1}
    {'h': 1, 'o': 2, 'w': 1, ' ': 1, 'l': 1, 'd': 1}
    {'h': 1, 'o': 2, 'w': 1, ' ': 2, 'l': 1, 'd': 1}
    {'h': 1, 'o': 2, 'w': 1, ' ': 2, 'l': 1, 'd': 1, 'a': 1}
    {'h': 1, 'o': 2, 'w': 1, ' ': 2, 'l': 1, 'd': 1, 'a': 1, 'r': 1}
    {'h': 1, 'o': 2, 'w': 1, ' ': 2, 'l': 1, 'd': 1, 'a': 1, 'r': 1, 'e': 1}
    {'h': 1, 'o': 2, 'w': 1, ' ': 3, 'l': 1, 'd': 1, 'a': 1, 'r': 1, 'e': 1}
    {'h': 1, 'o': 2, 'w': 1, ' ': 3, 'l': 1, 'd': 1, 'a': 1, 'r': 1, 'e': 1, 'y': 1}
    {'h': 1, 'o': 3, 'w': 1, ' ': 3, 'l': 1, 'd': 1, 'a': 1, 'r': 1, 'e': 1, 'y': 1}
    {'h': 1, 'o': 3, 'w': 1, ' ': 3, 'l': 1, 'd': 1, 'a': 1, 'r': 1, 'e': 1, 'y': 1, 'u': 1}
    {'h': 1, 'o': 3, 'w': 1, ' ': 3, 'l': 1, 'd': 1, 'a': 1, 'r': 1, 'e': 1, 'y': 1, 'u': 1, '?': 1}
    {'h': 1, 'o': 3, 'w': 1, ' ': 3, 'l': 1, 'd': 1, 'a': 1, 'r': 1, 'e': 1, 'y': 1, 'u': 1, '?': 2}
    {'h': 1, 'o': 3, 'w': 1, ' ': 3, 'l': 1, 'd': 1, 'a': 1, 'r': 1, 'e': 1, 'y': 1, 'u': 1, '?': 2}



```python
print(sorted(d.items(),key = lambda item: item[1], reverse = True))
```

    [('o', 3), (' ', 3), ('?', 2), ('h', 1), ('w', 1), ('l', 1), ('d', 1), ('a', 1), ('r', 1), ('e', 1), ('y', 1), ('u', 1)]


**关于lambda函数表达式**

**Python中，lambda函数也叫匿名函数，及即没有具体名称的函数，它允许快速定义单行函数，类似于C语言的宏，可以用在任何需要函数的地方。这区别于def定义的函数。**

[blog](https://blog.csdn.net/zjuxsl/article/details/79437563) 这个和下面的是一个东西！

https://blog.csdn.net/zjuxsl/article/details/79437563
