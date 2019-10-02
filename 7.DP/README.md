# 7.动态规划
1.0-1背包问题

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
                optp[i][j] = max(optp[i - 1][j], optp[i - 1][j - w[i]] + p[i])    # optp[i - 1][j]是上一个单元的值， optp[i - 1][j - w[i]]为剩余空间的价值
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
