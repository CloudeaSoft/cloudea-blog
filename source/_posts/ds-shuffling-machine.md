---
title: 算法题 - Shuffling Machine
date: 2024-06-05 22:22:52
categories: 数据结构与算法
tags:
  - C++
  - 数据结构
---

|                   |
| ----------------- |
| **Introduction:** |

Shuffling is a procedure used to randomize a deck of playing cards. Because standard shuffling techniques are seen as weak, and in order to avoid "inside jobs" where employees collaborate with gamblers by performing inadequate shuffles, many casinos employ automatic shuffling machines. Your task is to simulate a shuffling machine.<br /><br />The machine shuffles a deck of 54 cards according to a given random order and repeats for a given number of times. It is assumed that the initial status of a card deck is in the following order:<br /><br />S1, S2, ..., S13,<br /> H1, H2, ..., H13, <br />C1, C2, ..., C13, <br />D1, D2, ..., D13, <br />J1, J2<br /><br />where "S" stands for "Spade", "H" for "Heart", "C" for "Club", "D" for "Diamond", and "J" for "Joker". A given order is a permutation of distinct integers in [1, 54]. If the number at the i-th position is j, it means to move the card from position i to position j. For example, suppose we only have 5 cards: S3, H5, C1, D13 and J2. Given a shuffling order {4, 2, 5, 3, 1}, the result will be: J2, H5, D13, S3, C1. If we are to repeat the shuffling again, the result will be: C1, H5, S3, J2, D13.
**Input Specification:**
Each input file contains one test case. For each case, the first line contains a positive integer K (≤20) which is the number of repeat times. Then the next line contains the given order. All the numbers in a line are separated by a space.
**Output Specification:**
For each test case, print the shuffling results in one line. All the cards are separated by a space, and there must be no extra space at the end of the line.
**Sample Input:**
2<br />36 52 37 38 3 39 40 53 54 41 11 12 13 42 43 44 2 4 23 24 25 26 27 6 7 8 48 49 50 51 9 10 14 15 16 5 17 18 19 1 20 21 22 28 29 30 31 32 33 34 35 45 46 47
**Sample Output:**
S7 C11 C10 C12 S1 H7 H8 H9 D8 D9 S11 S12 S13 D10 D11 D12 S3 S4 S6 S10 H1 H2 C13 D2 D3 D4 H6 H3 D13 J1 J2 C1 C2 C3 C4 D1 S5 H5 H11 H12 C6 C7 C8 C9 S2 S8 S9 H10 D5 D6 D7 H4 H13 C5 |

##首先理解一下题目：
我们有一副有序的扑克牌（int[54]），需要拿到一组指令集（int[54]），和指令集执行次数（int），然后按照指令集交换卡牌的位置，最后输出卡牌的位置 ###输入：
1 个整数，1 个长度为 54 的数组 ###过程：

1. 生成一个长度为 54 的有序数组，数据从 1-54 按升序排列
2. 然后进行若干次循环，交换卡牌的位置 ###输出：
   打印出卡牌的“花色”和“数字”

###额外想法：

- 把卡牌抽象成 1-54，最后用字典来输出。这样可以简化卡牌的生成，专心实现卡牌交换
- 把卡牌数 54 抽象到一个固定变量 MAXSIZE，增加复用的可能

###抽象实现：

```
#include <stdio.h>
const int MAXSIZE = 54;

int initOrders(int orders[]);
int initCards(int cards[]);
int exchangeCards(int cards[], int orders[], int repeatCount);
void outputWithDictionary(int output);

int main()
{
    int cards[MAXSIZE];
    int orders[MAXSIZE];
    int repeatCount = 0;

    // Input count and orders
    scanf("%d", &repeatCount);
    initOrders(orders);

    // Initialize cards
    initCards(cards);

    // Process
    exchangeCards(cards, orders, repeatCount);

    // Output cards
    for (int i = 0; i < MAXSIZE; i++)
    {
        int output = cards[i];
        outputWithDictionary(output);
        if (i < MAXSIZE - 1)
        {
            printf(" ");
        }
    }
}
```

##然后是过程的具体实现 ###读取指令：for 循环

<details>
<summary>点击查看代码</summary>

```
int initOrders(int orders[])
{
    for (int i = 0; i < MAXSIZE; i++)
    {
        scanf("%d", &orders[i]);
    }
    // for (int i = 0; i < MAXSIZE; i++)
    // {
    //     printf("%d ", orders[i]);
    // }
    // printf("\n");
    return 0;
}
```

</details>

###初始化卡牌: for 循环

<details>
<summary>点击查看代码</summary>

```
int initCards(int cards[])
{
    for (int i = 0; i < MAXSIZE; i++)
    {
        cards[i] = i + 1;
    }
    // for (int i = 0; i < MAXSIZE; i++)
    // {
    //     printf("%d ", cards[i]);
    // }
    // printf("\n");
    return 0;
}
```

</details>

###交换卡牌：

- 使用了一个中介的数组，保存卡牌按照指令修改后的位置。然后用 for 循环依次扫描填入后，再将临时数组的内容放回卡牌数组中。复杂度 O(2N)=>O(N)

<details>
<summary>点击查看代码</summary>

```

int exchangeCards(int cards[], int orders[], int repeatCount)
{
    int temps[MAXSIZE] = {0};
    for (int i = 0; i < repeatCount; i++)
    {
        for (int j = 0; j < MAXSIZE; j++)
        {
            temps[orders[j] - 1] = cards[j];
        }
        for (int j = 0; j < MAXSIZE; j++)
        {
            cards[j] = temps[j];
        }
    }
    return 0;
}
```

</details>

###输出字典：

- 每 13 张更换一次色号——首字母，大小王各 1 张（J1，J2）。用 If 判断简单实现，switch 也行。

<details>
<summary>点击查看代码</summary>

```
void outputWithDictionary(int output)
{
    if (output <= 13)
    {
        printf("S%d", output);
    }
    else if (output <= 26)
    {
        printf("H%d", output - 13);
    }
    else if (output <= 39)
    {
        printf("C%d", output - 26);
    }
    else if (output <= 52)
    {
        printf("D%d", output - 39);
    }
    else if (output <= 54)
    {
        printf("J%d", output - 52);
    }
}
```

</details>

#完整代码

```
#include <stdio.h>
const int MAXSIZE = 54;

int initOrders(int orders[]);
int initCards(int cards[]);
int exchangeCards(int cards[], int orders[], int repeatCount);
void outputWithDictionary(int output);

int main()
{
    int cards[MAXSIZE];
    int orders[MAXSIZE];
    int repeatCount = 0;

    // Input count and orders
    scanf("%d", &repeatCount);
    initOrders(orders);

    // Initialize cards
    initCards(cards);

    // Process
    exchangeCards(cards, orders, repeatCount);

    // Output cards
    for (int i = 0; i < MAXSIZE; i++)
    {
        int output = cards[i];
        outputWithDictionary(output);
        if (i < MAXSIZE - 1)
        {
            printf(" ");
        }
    }
}

int initOrders(int orders[])
{
    for (int i = 0; i < MAXSIZE; i++)
    {
        scanf("%d", &orders[i]);
    }
    // for (int i = 0; i < MAXSIZE; i++)
    // {
    //     printf("%d ", orders[i]);
    // }
    // printf("\n");
    return 0;
}

int initCards(int cards[])
{
    for (int i = 0; i < MAXSIZE; i++)
    {
        cards[i] = i + 1;
    }
    // for (int i = 0; i < MAXSIZE; i++)
    // {
    //     printf("%d ", cards[i]);
    // }
    // printf("\n");
    return 0;
}

int exchangeCards(int cards[], int orders[], int repeatCount)
{
    int temps[MAXSIZE] = {0};
    for (int i = 0; i < repeatCount; i++)
    {
        for (int j = 0; j < MAXSIZE; j++)
        {
            temps[orders[j] - 1] = cards[j];
        }
        for (int j = 0; j < MAXSIZE; j++)
        {
            cards[j] = temps[j];
        }
    }
    return 0;
}

void outputWithDictionary(int output)
{
    if (output <= 13)
    {
        printf("S%d", output);
    }
    else if (output <= 26)
    {
        printf("H%d", output - 13);
    }
    else if (output <= 39)
    {
        printf("C%d", output - 26);
    }
    else if (output <= 52)
    {
        printf("D%d", output - 39);
    }
    else if (output <= 54)
    {
        printf("J%d", output - 52);
    }
}
```
