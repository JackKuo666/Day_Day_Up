# 7.动态规划
## 1. 0-1背包问题

`问题`：
容量为4的背包能装最值钱的东西是什么？总价多少？


| num | commodity | weight | value |
| --- | ----- | ----- | ----- |
| 1 | 吉他 | 1 | 1500 |
| 2 | 音响 | 4 | 3000 |
| 3 | 电脑 | 3 | 2000 |
| 4 | 手机 | 1 | 2000 |

```py

"""
1.0-1背包问题
动态规划
"""
# 这里使用了图解中的吉他，音箱，电脑，手机做的测试，数据保持一致
w = [0, 1, 4, 3, 1]   #n个物体的重量(w[0]无用)
p = [0, 1500, 3000, 2000, 2000]   #n个物体的价值(p[0]无用)
n = len(w) - 1   #计算n的个数
m = 4   #背包的载重量

x = []   #装入背包的物体，元素为True时，对应物体被装入(x[0]无用)
v = 0
#optp[i][j]表示在前i个物体中，能够装入载重量为j的背包中的物体的最大价值
optp = [[0 for col in range(m + 1)] for raw in range(n + 1)]
#optp 相当于做了一个n*m的全零矩阵的赶脚，n行为物件，m列为自背包载重量

def knapsack_dynamic(w, p, n, m, x):
    #计算optp[i][j]
    for i in range(1, n + 1):       # 物品一件件来
        for j in range(1, m + 1):   # j为子背包的载重量，寻找能够承载物品的子背包
            if (j >= w[i]):         # 当物品的重量小于背包能够承受的载重量的时候，才考虑能不能放进去
                optp[i][j] = max(optp[i - 1][j], optp[i - 1][j - w[i]] + p[i])   
                # optp[i - 1][j]是上一个单元的值， optp[i - 1][j - w[i]]为剩余空间的价值
            else:
                optp[i][j] = optp[i - 1][j]

    #递推装入背包的物体,寻找跳变的地方，从最后结果开始逆推
    j = m
    for i in range(n, 0, -1):
        if optp[i][j] > optp[i - 1][j]:
            x.append(i)
            j = j - w[i]

    #返回最大价值，即表格中最后一行最后一列的值
    v = optp[n][m]
    return v

print ('最大值为：' + str(knapsack_dynamic(w, p, n, m, x)))
print ('物品的索引：',x)

#最大值为：4000
#物品的索引： [4, 3]

```
## 2.回文数组
来自：https://www.nowcoder.com/practice/00fccaa8e30d414ab86b9bb229bd8e68

### 题目描述
对于一个给定的正整数组成的数组 a[] ，如果将 a 倒序后数字的排列与 a 完全相同，我们称这个数组为“回文”的。
例如， [1, 2, 3, 2, 1] 的倒序是他自己，所以是一个回文的数组；而 [1, 2, 3, 1, 2] 的倒序是 [2, 1, 3, 2, 1] ，所以不是一个回文的数组。
对于任意一个正整数数组，如果我们向其中某些特定的位置插入一些正整数，那么我们总是能构造出一个回文的数组。

输入一个正整数组成的数组，要求你插入一些数字，使其变为回文的数组，且数组中所有数字的和尽可能小。输出这个插入后数组中元素的和。

例如，对于数组 [1, 2, 3, 1, 2] 我们可以插入两个 1 将其变为回文的数组 [1, 2, 1, 3, 1, 2, 1] ，这种变换方式数组的总和最小，为 11 ，所以输出为 11 。

### 输入描述:
输入数据由两行组成： 第一行包含一个正整数 L ，表示数组 a 的长度。 第二行包含 L 个正整数，表示数组 a 。  对于 40% 的数据： 1 < L <= 100 达成条件时需要插入的数字数量不多于 2 个。  对于 100% 的数据： 1 < L <= 1,000 0 < a[i] <= 1,000,000 达成条件时需要插入的数字数量没有限制。

### 输出描述:
输出一个整数，表示通过插入若干个正整数使数组 a 回文后，数组 a 的数字和的最小值。

#### 示例1
输入
```
8
51 23 52 97 97 76 23 51
```
输出
```
598
```
### 原理：
1、 先找到最大回文子序列的和

2、 然后 2*sum - 最大回文子序列的和
### 代码：

```py
"""
 * Dynamic Programming
 *
 * State:
 *   dp[i][j]: 表示a[i],...,a[j]中的回文子序列的最大和
 *
 * Initial State:
 *   dp[i][i] = a[i]
 *
 * State Transition:
 *   if (a[i] == a[j]) dp[i][j] = dp[i + 1][j - 1] + 2 * a[i];
 *   else dp[i][j] = max(dp[i + 1][j], dp[i][j - 1]);

"""

n = int(input())
lines = input().strip().split(" ")
values = list(map(int, lines))
dp = [[0] * n for _ in range(n)]
s = 0

for i in range(n - 1, -1, -1):
    dp[i][i] = values[i]
    s += values[i]
    if i == n - 1:
        continue
    for j in range(i + 1, n):

        if values[i] == values[j]:
            dp[i][j] = dp[i + 1][j - 1] + values[i] * 2
        else:
            dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])
        # print(values,"\n")
        # for k in dp:
        #     print(k)
        # print("\n")

print(s * 2 - dp[0][n - 1])
```
参考：https://www.nowcoder.com/questionTerminal/00fccaa8e30d414ab86b9bb229bd8e68?f=discussion

## 3.todo
在线编程——动态规划常见的面试问题总结（Python）
https://blog.csdn.net/zichen_ziqi/article/details/82184495
