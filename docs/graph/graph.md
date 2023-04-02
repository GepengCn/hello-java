# 图 `graph`

## 思维导图

![](https://p.ipic.vip/7mh4vs.jpg)

## 特性

- 非线性数据结构
- 由顶点`vertex`和边`edge`组成
- 可抽象的表示为一组顶点$V$和一组边$E$的集合
- 邻接：当两顶点之间有边相连时，称此两顶点**邻接**
- 路径：从顶点`A`到顶点`B`走过的边构成的序列，被称为从`A`到`B`的**路径**
- 度：表示一个顶点具有多少条边。

!!! tip "链表、二叉树与图"

    $V={1,2,3,4,5}$

    $E=\{(1,2),(1,3),(1,4),(2,3),(2,4),(2,5),(4,5)\}$

    $G = {V,E}$

    ![](https://p.ipic.vip/y5adxw.png)

## 图的表现形式

!!! note "无向图"

    <div class="center-table" markdown>
    ![](https://p.ipic.vip/3tgdtw.png){width=275}
    </div>
!!! note "邻接链表"
    <div class="center-table" markdown>
    ![](https://p.ipic.vip/7u4txl.png){width=325}
    </div>
!!! note "邻接矩阵"
    <div class="center-table" markdown>
    ![](https://p.ipic.vip/qnz8dc.png){width=325}
    </div>

## 常见应用
<div class="center-table" markdown>
| 应用      | 顶点                 | 边| 图计算解决问题|
| ----------- | ------------------------------------ | ------------------------------------ | ------------------------------------ |
| 社交网络       | 用户  |好友关系|潜在好友推荐|
| 地铁线路       | 站点  |站点间的连通性|最短路线推荐|
| 太阳系       | 星体  |星体间的万有引力作用|行星轨道计算|
</div>

## 图基础操作

### 1. 基于邻接矩阵的实现

!!! quote "基于邻接矩阵的实现"

    设图的顶点总数为 $n$ ，则有：

    - **添加或删除边**：直接在邻接矩阵中修改指定边的对应元素即可，使用$O(1)$时间。而由于是无向图，因此需要同时更新两个方向的边。
    - **添加顶点**：在邻接矩阵的尾部添加一行一列，并全部填$0$即可，使用$O(n)$时间。
    - **删除顶点**：在邻接矩阵中删除一行一列。当删除首行首列时达到最差情况，需要将$(n-1)^2$个元素**向左上移动**，从而使用$O(n)^2$时间。
    - **初始化**：传入$n$个顶点，初始化长度为$n$的顶点列表`list`，使用$O(n)$时间；初始化 $n \times n$ 大小的邻接矩阵`matrix`，使用$O(n^2)$时间。

    === "1.1 初始化"

        ![](https://p.ipic.vip/9bhcac.png)

    === "1.2 添加边"

        ![](https://p.ipic.vip/u2m2xi.png)

    === "1.3 删除边"

        ![](https://p.ipic.vip/vb6yjx.png)

    === "1.4 添加顶点"

        ![](https://p.ipic.vip/qs7hmm.png)

    === "1.5 删除顶点"

        ![](https://p.ipic.vip/eontei.png)

### 2. 基于邻接链表的实现

!!! quote "基于邻接链表的实现"

    设图的顶点总数为 $n$ 、边总数为 $m$ ，则有：

    - **添加边**：在顶点对应链表的尾部添加边即可，使用 $O(1)$ 时间。因为是无向图，所以需要同时添加两个方向的边。
    - **删除边**：在顶点对应链表中查询与删除指定边，使用 $O(m)$ 时间。与添加边一样，需要同时删除两个方向的边。
    - **添加顶点**：在邻接表中添加一个链表即可，并以新增顶点为链表头结点，使用 $O(1)$ 时间。
    - **删除顶点**：需要遍历整个邻接表，删除包含指定顶点的所有边，使用 $O(m+n)$ 时间。
    - **初始化**：需要在邻接表中建立 $n$ 个结点和 $m$ 条边，使用 $O(n+m)$ 时间。

    === "2.1 初始化"

        ![](https://p.ipic.vip/ikl2w0.png)

    === "2.2 添加边"

        ![](https://p.ipic.vip/dpleuf.png)

    === "2.3 删除边"

        ![](https://p.ipic.vip/py6r18.png)

    === "2.4 添加顶点"

        ![](https://p.ipic.vip/neab7z.png)

    === "2.5 删除顶点"

        ![](https://p.ipic.vip/ffkp8g.png)

## 效率对比


设图中共有 $n$ 个顶点和 $m$ 条边，下表为邻接矩阵和邻接表的时间和空间效率对比。

<div class="center-table" markdown>

|              | 邻接矩阵 | 邻接表（链表） | 邻接表（哈希表） |
| ------------ | -------- | -------------- | ---------------- |
| 判断是否邻接 | $O(1)$   | $O(m)$         | $O(1)$           |
| 添加边       | $O(1)$   | $O(1)$         | $O(1)$           |
| 删除边       | $O(1)$   | $O(m)$         | $O(1)$           |
| 添加顶点     | $O(n)$   | $O(1)$         | $O(1)$           |
| 删除顶点     | $O(n^2)$ | $O(n + m)$     | $O(n)$           |
| 内存空间占用 | $O(n^2)$ | $O(n + m)$     | $O(n + m)$       |

</div>

观察上表，貌似邻接表（哈希表）的时间与空间效率最优。但实际上，在邻接矩阵中操作边的效率更高，只需要一次数组访问或赋值操作即可。总结以上，**邻接矩阵体现“以空间换时间”，邻接表体现“以时间换空间”**。


## 图的遍历

### 广度优先搜索


**广度优先搜索是一种由近及远的遍历方式，从距离最近的顶点开始访问，并一层层向外扩张**。具体地，从某个顶点出发，先遍历该顶点的所有邻接顶点，随后遍历下个顶点的所有邻接顶点，以此类推……

!!! info "算法实现"

    BFS 常借助「队列」来实现。队列具有**先入先出**的性质，这与 BFS **由近及远**的思想是异曲同工的。

    1. 将遍历起始顶点`startVet`加入队列，并开启循环；
    2. 在循环的每轮迭代中，弹出队首顶点弹出并记录访问，并将该顶点的所有邻接顶点加入到队列尾部；
    3. 循环 2. ，直到所有顶点访问完成后结束；

    为了防止重复遍历顶点，我们需要借助一个哈希表`visited`来记录哪些结点已被访问。

    === "1.1"

        ![](https://p.ipic.vip/q3vlzq.png)

    === "1.2"

        ![](https://p.ipic.vip/2996du.png)

    === "1.3"

        ![](https://p.ipic.vip/0db7cu.png)

    === "1.4"

        ![](https://p.ipic.vip/pmugrl.png)

    === "1.5"

        ![](https://p.ipic.vip/54tg6m.png)

    === "1.6"

        ![](https://p.ipic.vip/fqvg18.png)

    === "1.7"

        ![](https://p.ipic.vip/uknrzn.png)

    === "1.8"

        ![](https://p.ipic.vip/3cl6cv.png)

    === "1.9"

        ![](https://p.ipic.vip/bkddjn.png)

    === "1.10"

        ![](https://p.ipic.vip/q6dcev.png)

    ```java
    /* 广度优先遍历 BFS */
    // 使用邻接表来表示图，以便获取指定顶点的所有邻接顶点
    List<Vertex> graphBFS(GraphAdjList graph, Vertex startVet) {
        // 顶点遍历序列
        List<Vertex> res = new ArrayList<>();
            // 哈希表，用于记录已被访问过的顶点
        Set<Vertex> visited = new HashSet<>() {{ add(startVet); }};
        // 队列用于实现 BFS
        Queue<Vertex> que = new LinkedList<>() {{ offer(startVet); }};
            // 以顶点 vet 为起点，循环直至访问完所有顶点
        while (!que.isEmpty()) {
            Vertex vet = que.poll(); // 队首顶点出队
            res.add(vet);            // 记录访问顶点
            // 遍历该顶点的所有邻接顶点
            for (Vertex adjVet : graph.adjList.get(vet)) {
                if (visited.contains(adjVet))
                continue;        // 跳过已被访问过的顶点
                que.offer(adjVet);   // 只入队未访问的顶点
                visited.add(adjVet); // 标记该顶点已被访问
            }
        }
        // 返回顶点遍历序列
        return res;
    }

    ```

    - **时间复杂度：** 所有顶点都会入队、出队一次，使用 $O(|V|)$ 时间；在遍历邻接顶点的过程中，由于是无向图，因此所有边都会被访问 $2$ 次，使用 $O(2|E|)$ 时间；总体使用 $O(|V| + |E|)$ 时间。

    - **空间复杂度：** 列表 `res` ，哈希表 `visited` ，队列 `queue` 中的顶点数量最多为 $|V|$ ，使用 $O(|V|)$ 空间。

### 深度优先搜索

**深度优先搜索是一种优先走到底、无路可走再回头的遍历方式**。具体地，从某个顶点出发，不断地访问当前结点的某个邻接顶点，直到走到尽头时回溯，再继续走到底 + 回溯，以此类推……直至所有顶点遍历完成时结束。

!!! info "算法实现"

    这种“走到头 + 回溯”的算法形式一般基于递归来实现。与 BFS 类似，在 DFS 中我们也需要借助一个哈希表`visited`来记录已被访问的顶点，以避免重复访问顶点。

    === "2.1"

        ![](https://p.ipic.vip/cltc54.png)

    === "2.2"

        ![](https://p.ipic.vip/5buq6e.png)

    === "2.3"

        ![](https://p.ipic.vip/rcmn7x.png)

    === "2.4"

        ![](https://p.ipic.vip/6aspoh.png)

    === "2.5"

        ![](https://p.ipic.vip/fkpz4m.png)

    === "2.6"

        ![](https://p.ipic.vip/bykexk.png)

    === "2.7"

        ![](https://p.ipic.vip/5f9vdj.png)

    === "2.8"

        ![](https://p.ipic.vip/4m01ea.png)

    === "2.9"

        ![](https://p.ipic.vip/04918x.png)

    === "2.10"

        ![](https://p.ipic.vip/z9cuz6.png)



    ```java
    /* 深度优先遍历 DFS 辅助函数 */
    void dfs(GraphAdjList graph, Set<Vertex> visited, List<Vertex> res, Vertex vet) {
        res.add(vet);     // 记录访问顶点
        visited.add(vet); // 标记该顶点已被访问
        // 遍历该顶点的所有邻接顶点
        for (Vertex adjVet : graph.adjList.get(vet)) {
            if (visited.contains(adjVet))
            continue; // 跳过已被访问过的顶点
            // 递归访问邻接顶点
            dfs(graph, visited, res, adjVet);
        }
    }

    /* 深度优先遍历 DFS */
    // 使用邻接表来表示图，以便获取指定顶点的所有邻接顶点
    List<Vertex> graphDFS(GraphAdjList graph, Vertex startVet) {
        // 顶点遍历序列
        List<Vertex> res = new ArrayList<>();
            // 哈希表，用于记录已被访问过的顶点
        Set<Vertex> visited = new HashSet<>();
        dfs(graph, visited, res, startVet);
        return res;
    }

    ```