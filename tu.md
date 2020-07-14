# 图

## 1. 图的定义

表示“多对多”的关系

包含：

* 一组顶点：通常用 $$V(Vertex)$$ 表示顶点集合
* 一组边：通常用 $$E(Edge)$$ 表示边的集合
  * 边是顶点对： $$(v, w) \in E$$ ，其中 $$v, w \in V$$ 
  * 有向边 $$<v, w>$$ 表示从 $$v$$ 指向 $$w$$ 的边（单行线）
  * 不考虑重边和自回路

## 2. 抽象数据类型定义

* **类型名称：**图 $$(Graph)$$ 
* **数据对象集合：** $$G(V, E)$$ 由一个**非空**的有限顶点集合 $$V$$ 和一个有限边集合 $$E$$ 组成
* **操作集：**对于任意图 $$G \in Graph$$ ，以及 $$v \in V, e \in E$$ 
  * `Graph Create()` ：建立并返回空图；
  * `Graph InsertVertex(Graph G, Vertex v)` ：将 $$v$$ 插入 $$G$$ ；
  * `Graph InsertEdge(Graph G, Edge e)` ：将 $$e$$ 插入 $$G$$ ；
  * `void DFS(Graph G, Vertex v)` ：从顶点 $$v$$ 出发深度优先遍历图 $$G$$ ；
  * `void BFS(Graph G, Vertex v)` ：从顶点 $$v$$ 出发宽度优先遍历图 $$G$$ ；
  * `void ShortestPath(Graph G, Vertex v, int Dist[])` ：计算图 $$G$$ 中顶点 $$v$$ 到任意其它顶点的最短距离；
  * `void MST(Graph G)` ：计算图 $$G$$ 的最小生成树；
  * $$\cdots \cdots$$ 

## 3. 图的表示

### 3.1 邻接矩阵

邻接矩阵 $$G[N][N]$$ ： $$N$$ 个顶点从 0 到 N-1 编号

### 3.2 邻接表

