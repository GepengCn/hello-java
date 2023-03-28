# 红黑树

## 思维导图
![](https://p.ipic.vip/0c8jou.jpg)

## 特性

- 二叉搜索树
- 增加一个存储位表示结点颜色
- 没有一个路径会比其他路径长2倍，因而是近似于平衡的
- 从某个结点出发（不含该结点）到达一个叶结点的任意一条路径上的黑色结点个数称为该结点的**黑高**
- 红黑树的黑高等于其根结点的黑高
- 一颗有$n$个内部结点的红黑树的高度至多为 $2\lg (n+1)$
- 增删查的时间复杂度为$O(\lg(n))$
- 新插入的结点是红色


## 红黑树满足以下条件

1. 每个结点或是红色的，或是黑色的
2. 根结点是黑色的
3. 每个叶结点是黑色的
4. 如果一个结点是红色的，则它的两个子结点都是黑色的
5. 对每个结点，从该结点到其所有后代叶结点的简单路径上，均包含相同数目的黑色结点

## 常见应用

- `TreeMap` 和 `TreeSet` 都是基于红黑树实现的
- `JDK8` 中 `HashMap` 当链表长度大于 8 时会转化为红黑树

## 常用操作

### 插入

#### 1. 插入根结点

!!! quote "插入的新结点 N 是红黑树的根结点，这种情况下，我们把结点 N 的颜色由红色变为黑色，性质2（根是黑色）被满足。同时 N 被染成黑色后，红黑树所有路径上的黑色结点数量增加一个，性质5（从任一结点到其每个叶子的所有简单路径都包含相同数目的黑色结点）仍然被满足"

    ![](https://p.ipic.vip/uq7dvi.jpg){width=200}

#### 2. 父结点为黑色

!!! quote  "N 的父结点是黑色，这种情况下，性质4（每个红色结点必须有两个黑色的子结点）和性质5没有受到影响，不需要调整。"

    ![](https://p.ipic.vip/w8tdlj.jpg){width=175}

#### 3. 左倾染色

!!! quote "条件：新增结点1的叔叔结点4为红色。"

    新增结点1，相当于2-3树中在结点2上添加了一个结点，这个时候并不影响树高，只需要染色保持红黑树的规则即可。

    === "3.1"
        ![](https://p.ipic.vip/9u903u.jpg){width=300}

    === "3.2"
        ![](https://p.ipic.vip/ooluf3.jpg){width=300}

    === "3.3"
        ![](https://p.ipic.vip/nsxt94.jpg){width=300}

#### 4. 右倾染色

!!! quote "条件：新增结点4的叔叔结点1是红色。"

    新增结点4，相当于2-3树中在结点3上添加了一个结点，这个时候并不影响树高，只需要染色保持红黑树的规则即可。

    === "4.1"
        ![](https://p.ipic.vip/t57qmo.jpg){width=300}

    === "4.2"
        ![](https://p.ipic.vip/h7q78x.jpg){width=300}

    === "4.3"
        ![](https://p.ipic.vip/wapqo7.jpg){width=300}

#### 5. 左旋调衡

!!! quote "条件：新增结点的叔叔结点不是红色"

    对照2-3树，只有当一个结点内有3个结点的时候，才需要调衡。那么红黑树则是判断当前结点的叔叔结点是否为红色结点，如果不是则没法通过染色调衡，也就是需要选择进行调衡。

    - [x] 一次左旋

    === "5.1.1"
        ![](https://p.ipic.vip/hmxdju.jpg){width=275}

    === "5.1.2"
        ![](https://p.ipic.vip/jyy6hy.jpg){width=275}

    === "5.1.3"
        ![](https://p.ipic.vip/pu2aqy.jpg){width=225}

    - [x] 右旋 $+$ 左旋

    === "5.2.1"
        ![](https://p.ipic.vip/mtumg6.jpg){width=225}

    === "5.2.2"
        ![](https://p.ipic.vip/ww479o.jpg){width=275}

    === "5.2.3"
        ![](https://p.ipic.vip/jyy6hy.jpg){width=275}

    === "5.2.4"
        ![](https://p.ipic.vip/pu2aqy.jpg){width=225}

#### 6. 右旋调衡

!!! quote "条件：新增结点的叔叔结点不是红色"

    同左旋

    - [x] 一次右旋

    === "6.1.1"
        ![](https://p.ipic.vip/4hqwyf.jpg){width=275}

    === "6.1.2"
        ![](https://p.ipic.vip/f5vljj.jpg){width=275}

    === "6.1.3"
        ![](https://p.ipic.vip/bnhlvv.jpg){width=225}

    - [x] 左旋 $+$ 右旋

    === "6.2.1"
        ![](https://p.ipic.vip/t0htv1.jpg){width=225}

    === "6.2.2"
        ![](https://p.ipic.vip/7011w2.jpg){width=275}

    === "6.2.3"
        ![](https://p.ipic.vip/f5vljj.jpg){width=275}

    === "6.2.4"
        ![](https://p.ipic.vip/bnhlvv.jpg){width=225}

### 删除
#### 1. 删除根结点

!!! quote "删除的是根结点，则直接将根结点置为 `null`"

    ![](https://p.ipic.vip/9a554q.jpg){width=225}
#### 2. 左右子结点都为`null`

!!! quote "左右子结点都为`null`"

    === "2.1.1"

        ![](https://p.ipic.vip/u6p5x5.jpg){width=275}

    === "2.1.2"

        ![](https://p.ipic.vip/ui2hvx.jpg){width=232.5}
#### 3. 左右子结点有一个有值

!!! quote "待删除结点的左右子结点有一个有值，则用有值的结点替换该结点即可。"

    === "3.1.1"

        ![](https://p.ipic.vip/otftgh.jpg){width=232.5}

    === "3.1.2"

        ![](https://p.ipic.vip/pz1ljp.jpg){width=200}

#### 4. 前驱为黑色结点，并且有一个非 `null` 子结点

!!! quote "待删除结点的左右子结点有一个有值，则用有值的结点替换该结点即可。"

    1. 找到前驱结点
    2. 用前驱结点的值替换待删除结点的值
    3. 删除前驱结点
    4. 变色

    === "4.1.1 找到前驱"

        ![](https://p.ipic.vip/x3crua.jpg){width=375}

    === "4.1.2 用前驱赋值"

        ![](https://p.ipic.vip/a1ov8d.jpg){width=375}

    === "4.1.3 删除前驱"

        ![](https://p.ipic.vip/5ro0u2.jpg){width=275}

    === "4.1.4 变色"

        ![](https://p.ipic.vip/jz34kk.jpg){width=275}

#### 5. 前驱为黑色节点，同时子节点都为 `null`

!!! quote "前驱为黑色节点，同时子节点都为 `null`"

    1. 找到前驱结点
    2. 用前驱结点的值替换待删除结点的值
    3. 删除前驱结点
    4. 变色

    === "5.1.1 找到前驱"

        ![](https://p.ipic.vip/fcqv7c.jpg){width=375}

    === "5.1.2 用前驱赋值"

        ![](https://p.ipic.vip/k4ga7p.jpg){width=375}

    === "5.1.3 删除前驱"

        ![](https://p.ipic.vip/ywlzqs.jpg){width=300}

    === "5.1.4 变色"

        ![](https://p.ipic.vip/oneo2o.jpg){width=240}

#### 6. 前驱为红色结点，同时子结点都为`null`

!!! quote "前驱为红色结点，同时子结点都为`null`"

    1. 找到前驱结点
    2. 用前驱结点的值替换待删除结点的值
    3. 删除前驱结点

    === "6.1.1 找到前驱"
        ![](https://p.ipic.vip/mzek9z.jpg){width=375}

    === "6.1.2 用前驱赋值"
        ![](https://p.ipic.vip/zen2eg.jpg){width=375}

    === "6.1.3 删除前驱"
        ![](https://p.ipic.vip/0g0qmw.jpg){width=250}

!!! tip "补充"

    后继的情况与前驱类似

    1. 找到后继结点
    2. 用后继结点的值替换待删除结点的值
    3. 删除后继结点
    4. 变色

## 代码实现

=== "前置"

    ```java
    private static final boolean RED = true;
    private static final boolean BLACK = false;

    private class Node {
        Key key;//键
        Value val;//相关联的值
        Node left, right;//左右子树
        int N;//这棵树中的结点总数
        boolean color;//由其父结点指向它的链接的颜色

        Node(Key key, Value val, int N, boolean color){
            this.key = key;
            this.val = val;
            this.N = N;
            this.color = color;
        }
    }

    private boolean isRed(Node x){
        if(x==null)return false;
        return x.color == RED;
    }

    ```
=== "左旋"

    ```java
    Node rotateLeft(Node h){
        Node x = h.right;
        h.right = x.left;
        x.left = h;
        x.color = h.color;
        h.color = RED;
        x.N = h.N;
        h.N = 1 + size(h.left) + size(h.right);
        return x;
    }

    ```
=== "右旋"

    ```java
    Node rotateRight(Node h){
        Node x = h.left;
        h.left = x.right;
        x.right = h;
        x.color = h.color;
        h.color = RED;
        x.N = h.N;
        h.N = 1 + size(h.left) + size(h.right);
        return x;
    }
    ```
=== "插入算法"

    ```java

    public void put(Key key, Value val) {
        if (key == null) throw new IllegalArgumentException("first argument to put() is null");
        if (val == null) {
            delete(key);
            return;
        }

        root = put(root, key, val);
        root.color = BLACK;
    }

    // insert the key-value pair in the subtree rooted at h
    private Node put(Node h, Key key, Value val) {
        if (h == null) return new Node(key, val, RED, 1);

        int cmp = key.compareTo(h.key);
        if      (cmp < 0) h.left  = put(h.left,  key, val);
        else if (cmp > 0) h.right = put(h.right, key, val);
        else              h.val   = val;

        // fix-up any right-leaning links
        if (isRed(h.right) && !isRed(h.left))      h = rotateLeft(h);
        if (isRed(h.left)  &&  isRed(h.left.left)) h = rotateRight(h);
        if (isRed(h.left)  &&  isRed(h.right))     flipColors(h);
        h.size = size(h.left) + size(h.right) + 1;

        return h;
    }
    ```
=== "删除算法"

    ```java
    public void delete(Key key) {
        if (key == null) throw new IllegalArgumentException("argument to delete() is null");
        if (!contains(key)) return;

        // if both children of root are black, set root to red
        if (!isRed(root.left) && !isRed(root.right))
        root.color = RED;

        root = delete(root, key);
        if (!isEmpty()) root.color = BLACK;
    }

    // delete the key-value pair with the given key rooted at h
    private Node delete(Node h, Key key) {

        if (key.compareTo(h.key) < 0)  {
            if (!isRed(h.left) && !isRed(h.left.left))
            h = moveRedLeft(h);
            h.left = delete(h.left, key);
        }
        else {
            if (isRed(h.left))
            h = rotateRight(h);
            if (key.compareTo(h.key) == 0 && (h.right == null))
            return null;
            if (!isRed(h.right) && !isRed(h.right.left))
            h = moveRedRight(h);
            if (key.compareTo(h.key) == 0) {
                Node x = min(h.right);
                h.key = x.key;
                h.val = x.val;
                // h.val = get(h.right, min(h.right).key);
                // h.key = min(h.right).key;
                h.right = deleteMin(h.right);
            }
            else h.right = delete(h.right, key);
        }
        return balance(h);
    }
    ```