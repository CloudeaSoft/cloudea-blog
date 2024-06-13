---
title: 算法题 - Six Degrees of Separation
date: 2024-06-06 20:45:47
categories: 数据结构与算法
tags:
  - C++
  - 数据结构
---

“六度空间”理论又称作“六度分隔（Six Degrees of Separation）”理论。这个理论可以通俗地阐述为：“你和任何一个陌生人之间所间隔的人不会超过六个，也就是说，最多通过五个人你就能够认识任何一个陌生人。”如图 1 所示。

<div align="center">

![](/images/illustrations/ds-six-degrees-of-separation/1.jpg)
图 1 六度空间示意图
<br>

</div>

“六度空间”理论虽然得到广泛的认同，并且正在得到越来越多的应用。但是数十年来，试图验证这个理论始终是许多社会学家努力追求的目标。然而由于历史的原因，这样的研究具有太大的局限性和困难。随着当代人的联络主要依赖于电话、短信、微信以及因特网上即时通信等工具，能够体现社交网络关系的一手数据已经逐渐使得“六度空间”理论的验证成为可能。

假如给你一个社交网络图，请你对每个节点计算符合“六度空间”理论的结点占结点总数的百分比。

### 输入格式

输入第 1 行给出两个正整数，分别表示社交网络图的结点数 N（1<N≤10³，表示人数）、边数 M（≤33×N，表示社交关系数）。随后的 M 行对应 M 条边，每行给出一对正整数，分别是该条边直接连通的两个结点的编号（节点从 1 到 N 编号）。

### 输出格式

对每个结点输出与该结点距离不超过 6 的结点数占结点总数的百分比，精确到小数点后 2 位。每个结节点输出一行，格式为“结点编号:（空格）百分比%”。

<details>
<summary>输入样例</summary>

|      |
| ---- |
| 10 9 |
| 1 2  |
| 2 3  |
| 3 4  |
| 4 5  |
| 5 6  |
| 6 7  |
| 7 8  |
| 8 9  |
| 9 10 |

</details>

<details>
<summary>输出样例</summary>

|            |
| ---------- |
| 1: 70.00%  |
| 2: 80.00%  |
| 3: 90.00%  |
| 4: 100.00% |
| 5: 100.00% |
| 6: 100.00% |
| 7: 100.00% |
| 8: 90.00%  |
| 9: 80.00%  |
| 10: 70.00% |

</details>

## 题意理解

### 思考过程

> 因为要保存二维关系，选择图做数据结构，同时边数不多，因此使用邻接表来存储。
> 然后是过程拆解，包括：数据读入、图的生成、对于所有节点的“六度查找”3 个步骤。其中前两个比较基础，就此略过，着重思考单次“六度查找”的过程。

- 对于图的遍历，使用广度优先，更适合对于一层一层的搜索；而深度优先则很难做到遍历整
- 结果输出为节点数，在访问过程中要记录访问的节点数
- 因为有层数限制，在查找时需要知道当前在哪层，就必须在搜索过程中累计访问的层数（难点）

对于第三点，使用两个指针来记录：last 和 tail。其中 last 表示当前层的最后一个节点，而 tail 则保存下一层最后被填入的节点。当队列遍历到 last 时，则说明当前层已被访问完毕，后续不会再有下一层的节点进入队列，因此当前的 tail 就是下一层的最后一个节点，将 tail 赋值给 last，不断循环。

### 伪代码

```C++
// 查找指定节点的六度节点
int SDS_BFS(Vertex V)
{
    // 确认节点是否被访问过
    visited[V] = { true };
    // 节点计数
    count = 0;
    // 层数计数
    level = 0;
    // 保存当前层的最后一个节点
    last = V;
    // 临时存储下一层的节点，当遍历到last时，赋值给last
    tail = V;
    // BFS用的队列，并将初始节点填入
    Q.Enqueue(V)
    while(!Q.IsEmpty())
    {
        V = Q.Dequeue();
        foreach(AdjNode W){
            if(!visited[W]){
                visited[W] = true;
                Q.Enqueue(W);
                count++;
                tail = W;
            }
        }

        // 是当前层的最后一个节点
        if (V == last) {
	        level++;
	        last = tail; // tail是下一层的最后一个节点
        }

        if (level == 6) {
        	break;
        }
    }

    return count;
}
```

## 实现代码

<details>
<summary>点击查看代码</summary>

```C++
#include <iostream>
#include <queue>
#include <iomanip>
using namespace std;

// Vertex Type
typedef int Vertex;

// Edge Node
struct ENode {
	Vertex V1, V2;
};
typedef ENode* Edge;

// Adjacent Node
struct AdjVNode {
	Vertex AdjV;
	AdjVNode* Next;
};

// Head Element of adjacent node list
typedef struct VNode {
	AdjVNode* FirstEdge;
} AdjList[1001];

// Graph Node
struct GNode {
	int Nv;
	int Ne;
	AdjList Map;
};
typedef GNode* Graph;

Graph BuildGraph();
Graph CreateGraph(int VertexNum);
void InsertEdge(Graph G, Edge E);
void SDS(Graph G);
int SDS_BFS(Graph G, Vertex V);

int main()
{
	Graph G = BuildGraph();
	SDS(G);
}

Graph BuildGraph()
{
	Graph G;
	int Nv;

	cin >> Nv;
	G = CreateGraph(Nv);

	cin >> G->Ne;
	Edge E = new struct ENode;
	for (int i = 0; i < G->Ne; i++) {
		cin >> E->V1 >> E->V2;
		InsertEdge(G, E);
	}

	return G;
}

Graph CreateGraph(int VertexNum)
{
	Graph G = new struct GNode;
	G->Nv = VertexNum;
	G->Ne = 0;
	for (Vertex V = 1; V <= G->Nv; V++)
		G->Map[V].FirstEdge = NULL;

	return G;
}

void InsertEdge(Graph G, Edge E)
{
	AdjVNode* NewNode1 = new struct AdjVNode;
	NewNode1->AdjV = E->V2;
	NewNode1->Next = G->Map[E->V1].FirstEdge;
	G->Map[E->V1].FirstEdge = NewNode1;

	AdjVNode* NewNode2 = new struct AdjVNode;
	NewNode2->AdjV = E->V1;
	NewNode2->Next = G->Map[E->V2].FirstEdge;
	G->Map[E->V2].FirstEdge = NewNode2;
}

void SDS(Graph G)
{
	bool isFirst = true;
	for (int i = 1; i <= G->Nv; i++) {
		int count = SDS_BFS(G, i);
		if (!isFirst) {
			cout << endl;
		}
		else {
			isFirst = false;
		}
		cout << i << ": " << fixed << setprecision(2) << 1.0000f * count * 100 / G->Nv << "%";
	}
}

int SDS_BFS(Graph G, Vertex V)
{
	bool* visited = new bool[G->Nv + 1];
	for (int i = 1; i <= G->Nv; i++) {
		visited[i] = false;
	}
	queue<int> Q = queue<int>();

	int level = 0, count = 0;
	Vertex last = V, tail = V;

	Q.push(V);

	AdjVNode* ptr;
	while (!Q.empty()) {
		V = Q.front();
		Q.pop();

		ptr = G->Map[V].FirstEdge;
		while (ptr) {
			if (!visited[ptr->AdjV]) {
				visited[ptr->AdjV] = true;
				Q.push(ptr->AdjV);
				count++;
				tail = ptr->AdjV;
			}
			ptr = ptr->Next;
		}

		if (V == last) {
			level++;
			last = tail;
		}

		if (level == 6) {
			break;
		}
	}
	return count;
}
```

</details>
