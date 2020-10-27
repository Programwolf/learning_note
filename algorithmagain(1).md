# 枚举
## **简介**

枚举（英语：Enumerate）是基于已有知识来猜测答案的一种问题求解策略。

枚举的思想是不断地猜测，从可能的集合中一一尝试，然后再判断题目的条件是否成立。

## **要点**
**给出解空间**

建立简洁的数学模型。

枚举的时候要想清楚：可能的情况是什么？要枚举哪些要素？

**减少枚举的空间**

枚举的范围是什么？是所有的内容都需要枚举吗？

在用枚举法解决问题的时候，一定要想清楚这两件事，否则会带来不必要的时间开销。

**选择合适的枚举顺序**

根据题目判断。比如例题中要求的是最大的符合条件的素数，那自然是从大到小枚举比较合适。

## **练习**

[2811: 熄灯问题 - OpenJudge](http://bailian.openjudge.cn/practice/2811/)

**描述**

有一个由按钮组成的矩阵，其中每行有6个按钮，共5行。每个按钮的位置上有一盏灯。当按下一个按钮后，该按钮以及周围位置(上边、下边、左边、右边)的灯都会改变一次。即，如果灯原来是点亮的，就会被熄灭；如果灯原来是熄灭的，则会被点亮。在矩阵角上的按钮改变3盏灯的状态；在矩阵边上的按钮改变4盏灯的状态；其他的按钮改变5盏灯的状态。
![BMnLmn.gif](https://s1.ax1x.com/2020/10/27/BMnLmn.gif)

在上图中，左边矩阵中用X标记的按钮表示被按下，右边的矩阵表示灯状态的改变。对矩阵中的每盏灯设置一个初始状态。请你按按钮，直至每一盏等都熄灭。与一盏灯毗邻的多个按钮被按下时，一个操作会抵消另一次操作的结果。在下图中，第2行第3、5列的按钮都被按下，因此第2行、第4列的灯的状态就不改变。

![BMnOwq.gif](https://s1.ax1x.com/2020/10/27/BMnOwq.gif)
请你写一个程序，确定需要按下哪些按钮，恰好使得所有的灯都熄灭。根据上面的规则，我们知道

（1）第2次按下同一个按钮时，将抵消第1次按下时所产生的结果。因此，每个按钮最多只需要按下一次；

（2）各个按钮被按下的顺序对最终的结果没有影响；

（3）对第1行中每盏点亮的灯，按下第2行对应的按钮，就可以熄灭第1行的全部灯。

如此重复下去，可以熄灭第1、2、3、4行的全部灯。同样，按下第1、2、3、4、5列的按钮，可以熄灭前5列的灯。

**输入**

5行组成，每一行包括6个数字（0或1）。相邻两个数字之间用单个空格隔开。0表示灯的初始状态是熄灭的，1表示灯的初始状态是点亮的。

**输出**

5行组成，每一行包括6个数字（0或1）。相邻两个数字之间用单个空格隔开。其中的1表示需要把对应的按钮按下，0则表示不需要按对应的按钮。

**样例输入**
```
0 1 1 0 1 0
1 0 0 1 1 1
0 0 1 0 0 1
1 0 0 1 0 1
0 1 1 1 0 0
```
**样例输出**
```
1 0 1 0 0 1
1 1 0 1 0 1
0 0 1 0 1 1
1 0 0 1 0 0
0 1 0 0 0 0
```

**代码**
```c++
//熄灯问题
#include<iostream>
#include<string>
#include<cstring>
using namespace std;

int getBit(char c, int i)//取c的第i位
{
    return (c >> i) & 1;
}
void setBit(char & c, int i, int v)//设置c的第i位为v
{
    if (v == 1) c |= (1 << i);
    else c &= ~(1 << i);
}
void flip(char & c, int i)//将c的第i位取反
{
    c ^= (1 << i);
}
void outputResult(int t, char result[])//输出结果
{
    // cout << "PUZZLE #" << t << endl;
    for (int i = 0; i < 5; ++i)
    {    
        for (int j = 0; j < 6; ++j)
        {
            cout << getBit(result[j], i);
            if (j < 5) cout << ' ';
        }
        cout << endl;
    }    
}

int main()
{
    char oriLights[6]; //初始灯矩阵
    char lights[6]; //变化的灯阵
    char result[6];
    char switchs; //某一行的开关状态
    int T;
    // cin >> T;
    T = 1;
    for (int t = 0; t < T; ++t)
    {
        memset(oriLights, 0, sizeof(oriLights));
        for (int i = 0; i < 5; ++i)//读入最初灯状态
        {
            for (int j = 0; j < 6; ++j)
            {
                int s;
                cin >> s;
                setBit(oriLights[j], i, s);
            }
        }

        for (int n = 0; n < 32; ++n)//遍历首列开关的32种操作
        {
            memcpy(lights, oriLights, sizeof(oriLights));
            switchs = n;//假定首列的开关需要的操作方案

            for (int j = 0; j < 6; ++j)
            {
                result[j] = switchs;//保存第j列开关的操作方案

                for (int i = 0; i < 5; ++i)
                {
                    if (getBit(switchs, i) == 1)
                    {
                        if (i > 0) flip(lights[j], i - 1);//改上灯
                        flip(lights[j], i);
                        if (i < 4) flip(lights[j], i + 1);//改下灯
                    }
                }
                if (j < 5) lights[j + 1] ^= switchs;//改下一列的灯

                switchs = lights[j];//第j+1列开关的操作方案由第j列的灯的状态决定
            }
            if(lights[5] == 0)
            {
                outputResult(t, result);
                break;
            }
        }//for (int n = 0; n < 32; ++n)
    }// for (int t = 0; t < T; ++t)

    return 0;
}
```
---
# 模拟
## 简介

模拟就是用计算机来模拟题目中要求的操作。

模拟题目通常具有**码量大、操作多、思路繁复**的特点。由于它码量大，经常会出现难以查错的情况，如果在考试中写错是相当浪费时间的。

## 技巧

写模拟题时，遵循以下的建议有可能会提升做题速度：

- 在动手写代码之前，在草纸上尽可能地写好要实现的流程。
- 在代码中，尽量把每个部分模块化，写成函数、结构体或类。
- 对于一些可能重复用到的概念，可以统一转化，方便处理：如，某题给你 "YY-MM-DD 时：分" 把它抽取到一个函数，处理成秒，会减少概念混淆。
- 调试时分块调试。模块化的好处就是可以方便的单独调某一部分。
- 写代码的时候一定要思路清晰，不要想到什么写什么，要按照落在纸上的步骤写。

实际上，上述步骤在解决其它类型的题目时也是很有帮助的。

## 练习

[Climbing Worm - HDU](http://acm.hdu.edu.cn/showproblem.php?pid=1049)

**Problem Description**

An inch worm is at the bottom of a well n inches deep. It has enough energy to climb u inches every minute, but then has to rest a minute before climbing again. During the rest, it slips down d inches. The process of climbing and resting then repeats. How long before the worm climbs out of the well? We'll always count a portion of a minute as a whole minute and if the worm just reaches the top of the well at the end of its climbing, we'll assume the worm makes it out.
 

**Input**

There will be multiple problem instances. Each line will contain 3 positive integers n, u and d. These give the values mentioned in the paragraph above. Furthermore, you may assume d < u and n < 100. A value of n = 0 indicates end of output.
 

**Output**

Each input instance should generate a single integer on a line, indicating the number of minutes it takes for the worm to climb out of the well.
 

**Sample Input**
```
10 2 1
20 3 1
0 0 0
```

**Sample Output**
```
17
19
```

**Code**
```c++
#include<iostream>

using namespace std;

int main()
{
    int n, u, d;
    int ans = 0;
    while (cin >> n >> u >> d)
    {
        if (n == 0)
            return 0;
        ans = 0;
        while (n > u)
        {
            n -= u;
            n += d;
            ans += 2;
        }
        ans++;
        cout << ans << endl;
    }

    return 0;
}
```

[【NOIP2014】生活大爆炸版石头剪刀布 - Universal Online Judge](https://uoj.ac/problem/15)

石头剪刀布是常见的猜拳游戏:石头胜剪刀,剪刀胜布,布胜石头。如果两个人出拳一 样，则不分胜负。在《生活大爆炸》第二季第 8 集中出现了一种石头剪刀布的升级版游戏。

升级版游戏在传统的石头剪刀布游戏的基础上,增加了两个新手势:

斯波克:《星际迷航》主角之一。

蜥蜴人:《星际迷航》中的反面角色。

这五种手势的胜负关系如表一所示,表中列出的是甲对乙的游戏结果。

甲\乙	|剪刀	|石头	|布	|蜥蜴人	|斯波克
--|--|--|--|--|--
剪刀	|平	|输	|赢	|赢	|输
石头	|×	|平	|输	|赢	|输
布	    |×	|×	|平	|输	|赢
蜥蜴人	|×	|×	|×	|平	|赢
斯波克	|×	|×	|×	|×	|平

现在,小 A 和小 B 尝试玩这种升级版的猜拳游戏。已知他们的出拳都是有周期性规律的，但周期长度不一定相等。例如：如果小A以“石头-布-石头-剪刀-蜥蜴人-斯波克”长度为 6 的周期出拳,那么他的出拳序列就是“石头-布-石头-剪刀-蜥蜴人-斯波克-石头-布-石头-剪刀-蜥蜴人-斯波克-......”,而如果小B以“剪刀-石头-布-斯波克-蜥蜴人”长度为 5 的周期出拳,那么他出拳的序列就是“剪刀-石头-布-斯波克-蜥蜴人-剪刀-石头-布-斯波克-蜥蜴人-......”

已知小 A 和小 B 一共进行 N 次猜拳。每一次赢的人得 1 分，输的得 0 分；平局两人都得 0 分。现请你统计 N 次猜拳结束之后两人的得分。

**输入格式**

第一行包含三个整数：N,NA,NB,分别表示共进行 N 次猜拳、小 A 出拳的周期长度，小 B 出拳的周期长度。数与数之间以一个空格分隔。

第二行包含 NA 个整数,表示小 A 出拳的规律,第三行包含 NB 个整数,表示小 B 出拳的规律。其中，0 表示“剪刀”，1 表示“石头”，2 表示“布”，3 表示“蜥蜴人”，4 表示“斯波克”。数与数之间以一个空格分隔。

**输出格式**

输出一行，包含两个整数，以一个空格分隔，分别表示小 A、小 B 的得分。

**样例一**
```
input
10 5 6
0 1 2 3 4
0 3 4 2 1 0

output
6 2
```
**样例二**
```
input
9 5 5
0 1 2 3 4
1 0 3 2 4

output
4 4
```
**限制与约定**
```
0<N≤200,0<NA≤200,0<NB≤200
时间限制：1s
内存限制：128MB
```

**代码**
```c++
// 【NOIP2014】生活大爆炸版石头剪刀布

#include<iostream>
#include<vector>

using namespace std;

int main()
{
    int n, na, nb;
    cin >> n >> na >> nb;
    vector<int> a(na);
    vector<int> b(nb);
    for (int i = 0; i < na; ++i)
    {
        cin >> a[i];
    }
    for (int i = 0; i < nb; ++i)
    {
        cin >> b[i];
    }

    int x = 0, y = 0;
    int A, B;
    int ansA = 0, ansB = 0;
    for (int i = 0; i < n; ++i)
    {
        A = a[x];
        B = b[y];
        // 写else if 是最烦的时候，这个时候一定要认真，争取不出错
        if (A == 0 && (B == 2 || B == 3))
            ansA++;
        else if (A == 0 && A != B)
            ansB++;
        else if (A == 1 && (B == 3 || B == 0))
            ansA++;
        else if (A == 1 && A != B)
            ansB++;
        else if (A == 2 && (B == 4 || B == 1))
            ansA++;
        else if (A == 2 && A != B)
            ansB++;
        else if (A == 3 && (B == 4 || B == 2))
            ansA++;
        else if (A == 3 && A != B)
            ansB++;
        else if (A == 4 && (B == 0 || B == 1))
            ansA++;
        else if (A == 4 && A != B)
            ansB++;
        x = (x + 1) % na;
        y = (y + 1) % nb;
    }

    cout << ansA << ' ' << ansB << endl;
    return 0;
}
```