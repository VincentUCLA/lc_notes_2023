### Stack & Queues

#### 堆栈与DFS和BFS

1. Stack一般用来模拟DFS，但不如回溯简洁
2. Queue一般用来模拟BFS

#### Infix Expression Evaluation

中缀到后缀转换：要点在于，为什么遇到计算符时要把优先级大于等于自己的运算符全部弹出？因为在后缀表达式里他们放在前面就等于先去计算他们啊；栈在这里实际上就是起到的一个保存计算顺序的作用，而“有控制地”从栈中弹出运算符可以达到把中缀式代换成语法树（也就是后缀式）的目的。

相关leetcode题目：150. Evaluate Reverse Polish Notation, 224. Basic Calculator

```py
def tokenize(self, str):
    newS = str.replace("(", " ( ")
    newS = newS.replace(")", " ) ")
    newS = newS.replace("+", " + ")
    newS = newS.replace("-", " - ")
    newS = newS.replace("*", " * ")
    newS = newS.replace("/", " / ")
    return newS.split(' ')


def optr(self, char):
    if char == '*' or char == '/':
        return 2
    elif char == '+' or char == '-':
        return 1
    else:
        return 0


def in2post(self, str):
    token = self.tokenize(str)
    stk = []
    ret = []
    for i in token:
        if i.isdecimal():
            ret.append(i)
        elif i == '(':
            stk.append(i)
        elif self.optr(i) > 0:
            if len(stk) == 0:
                stk.append(i)
            else:
                top = stk[len(stk) - 1]
                while self.optr(top) >= self.optr(i):
                    ret.append(top)
                    top = stk.pop()
                stk.append(i)
        elif i == ')':
            top = stk[len(stk) - 1]
            while top != '(':
                ret.append(top)
                top = stk.pop()
    while len(stk) > 0:
        ret.append(stk.pop())
    return ret
```

计算后缀式：这个不难，唯一需要注意的部分是两个操作数的弹出顺序。

```py
def cal(self, optr, opte1, opte2):
    opt1 = int(opte1)
    opt2 = int(opte2)
    if optr == '+':
        return opt1 + opt2
    elif optr == '-':
        return opt1 - opt2
    elif optr == '*':
        return opt1 * opt2
    elif optr == '/':
        return opt1 / opt2
    else:
        exit(1)


def eval(self, str):
    stk = []
    post = self.in2post(str)
    for i in post:
        if i.isdecimal():
            stk.append(i)
        else:
            opte2 = stk.pop()
            opte1 = stk.pop()
            stk.append(self.cal(i, opte1, opte2))
    return stk.pop()
```
