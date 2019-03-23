# <center>Python：每日一题20190323</center>

<div align=center><img src = "Balls.jpg"></div>

> **小球从最高处逐层落下，每个节点都有可能向左下或右下方向下落，且几率相同，各占50%，共有10万个小球依次落下，当都从第0层落至第9层时图中0~9的10个位置各有多少个小球（这里为了与python一致，都是从0开始的）。**

> * 第一种方法：

```python
import random
import numba as nb

@nb.jit
def fun(n):
    for i in range(n):
        summ = 0
        for j in range(numfloor-1):
            prob = random.randint(0, 1)
            summ += prob
        result[summ] += 1
    return result

numfloor = eval(input('输入你想要小球落下几层：'))
result = [0 for i in range(numfloor)]
test = fun(1000000)
for i, each in enumerate(test):
    print(i, '-->', each)
```

> * 第二种方法：
> 详见 https://fishc.com.cn/thread-102985-1-1.html

```python
class balls: #定义类
        def __init__(self): #初始化测试环境
                self.matrix = [[0]*i for i in range(1,11)]
                self.matrix[0][0] = 1 #把小球放在最顶上
        def roll(self,row,col): #定义一次小球从row行滚落到row+1行的动作
                if row>=9: return
                import random
                if random.random()>=0.5:
                        self.matrix[row][col] = 0
                        self.matrix[row+1][col+1] = 1
                else:
                        self.matrix[row][col] = 0
                        self.matrix[row+1][col] = 1
        def start(self): #定义小球从顶层滚落到底层的动作
                for r in range(10):
                        for c in range(r+1):
                                if self.matrix[r][c]==1:
                                        self.roll(r,c)
                                        break
                return sum(k*v for k,v in enumerate(self.matrix[9])) #返回小球在最底层的哪一格
result = [0]*10 #存放结果
for repeat in range(100000): #重复10万次
        test = balls()
        result[test.start()] += 1
print(result)
```