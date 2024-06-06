---
title: 算法题 - Pop Sequence
date: 2024-06-05 22:24:39
categories: 数据结构与算法
tags:
  - C++
  - 数据结构
---

Given a stack which can keep M numbers at most. Push N numbers in the order of 1, 2, 3, ..., N and pop randomly. You are supposed to tell if a given sequence of numbers is a possible pop sequence of the stack. For example, if M is 5 and N is 7, we can obtain 1, 2, 3, 4, 5, 6, 7 from the stack, but not 3, 2, 1, 7, 5, 6, 4.

**Input Specification:**

Each input file contains one test case. For each case, the first line contains 3 numbers (all no more than 1000): M (the maximum capacity of the stack), N (the length of push sequence), and K (the number of pop sequences to be checked). Then K lines follow, each contains a pop sequence of N numbers. All the numbers in a line are separated by a space.

**Output Specification:**

For each pop sequence, print in one line "YES" if it is indeed a possible pop sequence of the stack, or "NO" if not.

**Sample Input:**

5 7 5
1 2 3 4 5 6 7
3 2 1 7 5 6 4
7 6 5 4 3 2 1
5 6 4 3 7 2 1
1 7 6 5 4 3 2

**Sample Output:**

YES
NO
NO
YES
NO

---

### **题目简述：**

一个长度为 M 的堆栈，按照 1~N(M 可能小于 N)的顺序往其中填入数字, 可以在任意时刻 pop 出一个数字。现在给定一个序列, 问这个序列是不是一个可能的出栈序列。

### 处理思路：

1. 因为堆栈长度与数字数量不同，因此需要保证当前栈内的元素数量不超过 M 个。
2. 因为在一个元素出栈时，在这个元素之前的所有元素（比它小的那些数字），必定已经出栈或在栈内；如果在栈内，则这些元素必然按照后到前的顺序排列（它后面未出栈的，比它小的数字，必然按照大到小的顺序排列）。
3. 当前栈的元素数量 = 比某个数小的所有未出栈的数。
4. 输入部分，先开 3 个变量存放，然后开一个二维数组存放；输出部分，开一个 01 表示的结果数组，最后用 3 元表达式输出。

## 实现代码：

<details>
<summary>点击查看代码</summary>

```
#include <stdio.h>

void Solution(int maxCapacity, int length, int seqNum);

int main()
{
    int maxCapacity, length, seqNum;
    scanf("%d%d%d", &maxCapacity, &length, &seqNum);
    Solution(maxCapacity, length, seqNum);
}

void Solution(int maxCapacity, int length, int seqNum)
{
    int sequcences[seqNum][length];

    for (int i = 0; i < seqNum; i++)
    {
        for (int j = 0; j < length; j++)
        {
            scanf("%d", &sequcences[i][j]);
        }
    }

    int res[seqNum] = {0};
    for (int i = 0; i < seqNum; i++)
    {
        // 扫描这行的每个数
        for (int j = 0; j < length; j++)
        {
            int temp = sequcences[i][j];
            int count = 1;
            // 每个数后比他小的数必须递减排列
            for (int k = j; k < length; k++)
            {
                if (sequcences[i][k] >= sequcences[i][j])
                {
                }
                else if (sequcences[i][k] < temp)
                {
                    count++;
                    temp = sequcences[i][k];
                }
                else
                {
                    res[i] = 1;
                }
            }

            if (count > maxCapacity)
            {
                res[i] = 1;
            }
            if (res[i] != 0)
            {
                break;
            }
        }
    }

    for (int i = 0; i < seqNum - 1; i++)
    {
        printf("%s\n", res[i] == 0 ? "YES" : "NO");
    }

    printf("%s", res[seqNum - 1] == 0 ? "YES" : "NO");
}
```

</details>

### 写在后面的一些小牢骚

其实一开始就是用这种方法做的，但一直不对，却又搞不清楚是哪里出了问题。于是先想要换别的方法做，后面看到其他人也用类似的方法做过，于是决定继续研究，检查了半天才发现是输出结果的最后一行代码指针少写了一个"-1"。
不过这种方法可能不太好，时间复杂度高了一级（多扫描了一轮），还额外占用了不少空间（用一个二维数组，还有一个结果数组，网上其他的方法似乎都挺简单）。
