# 图 `graph`

## 特性

- 非线性数据结构
- 由顶点`vertex`和边`edge`组成
- 可抽象的表示为一组顶点$V$和一组边$E$的集合
- 邻接：当两顶点之间有边相连时，称此两顶点**邻接**
- 路径：从顶点`A`到顶点`B`走过的边构成的序列，被称为从`A`到`B`的“路径”
- 度：表示一个顶点具有多少条边。

!!! tip "比较链表与树"

    $V={1,2,3,4,5}$

    $E=\{(1,2),(1,3),(1,4),(2,3),(2,4),(2,5),(4,5)\}$

    $G = {V,E}$

    ![](https://p.ipic.vip/vvxes0.jpg)

## 图的表现形式

!!! note "无向图"

    <div class="center-table" markdown>
    ![](https://p.ipic.vip/bedjb5.jpg){width=275}
    </div>
!!! note "邻接链表"
    <div class="center-table" markdown>
    ![](https://p.ipic.vip/f9f9k9.jpg){width=325}
    </div>
!!! note "邻接矩阵"
    <div class="center-table" markdown>
    ![](https://p.ipic.vip/jrdlec.jpg){width=325}
    </div>

## 常见应用
<div class="center-table" markdown>
| 应用      | 顶点                 | 边| 图计算解决问题|
| ----------- | ------------------------------------ | ------------------------------------ | ------------------------------------ |
| 社交网络       | 用户  |好友关系|潜在好友推荐|
| 地铁线路       | 站点  |站点间的连通性|最短路线推荐|
| 太阳系       | 星体  |星体间的万有引力作用|行星轨道计算|
</div>