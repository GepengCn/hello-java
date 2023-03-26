# 平衡二叉搜索树`AVL树`

## 思维导图

![](https://p.ipic.vip/wiun6x.jpg)

## 特性

- 不断添加与删除结点后，AVL 树仍然不会发生退化，时间复杂度均能保持在$O(\log n)$
- 每个结点的左子树和右子树的高度差$\leq 1$的二叉搜索树
- 通过**旋转**恢复平衡

## 旋转

### 前置函数


=== "树节点类"

    ```java
    /* AVL 树结点类 */
    class TreeNode {
        public int val;        // 结点值
        public int height;     // 结点高度
        public TreeNode left;  // 左子结点
        public TreeNode right; // 右子结点
        public TreeNode(int x) { val = x; }
    }

    ```
=== "获取结点高度"

    ```java
    /* 获取结点高度 */
    int height(TreeNode node) {
        // 空结点高度为 -1 ，叶结点高度为 0
        return node == null ? -1 : node.height;
    }
    ```

=== "更新结点高度"

    ```java
    /* 更新结点高度 */
    void updateHeight(TreeNode node) {
        // 结点高度等于最高子树高度 + 1
        node.height = Math.max(height(node.left), height(node.right)) + 1;
    }
    ```

=== "获取平衡因子"

    ```java
    /* 获取平衡因子 */
    int balanceFactor(TreeNode node) {
        // 空结点平衡因子为 0
        if (node == null) return 0;
        // 结点平衡因子 = 左子树高度 - 右子树高度
        return height(node.left) - height(node.right);
    }

    ```

### 右旋
!!! question "右旋"

    **如果失衡结点的子树度为1**

    === "1."

        ![](https://p.ipic.vip/boksji.jpg)

    === "2."

        ![](https://p.ipic.vip/7bwqix.jpg)

    === "3."

        ![](https://p.ipic.vip/gl7s9p.jpg)

    === "4."

        ![](https://p.ipic.vip/unah0i.jpg)

    **如果失衡结点的子树度为2**

    ![](https://p.ipic.vip/u8t6wr.jpg)

```java
/* 右旋操作 */
TreeNode rightRotate(TreeNode node) {
    TreeNode child = node.left;
    TreeNode grandChild = child.right;
    // 以 child 为原点，将 node 向右旋转
    child.right = node;
    node.left = grandChild;
    // 更新结点高度
    updateHeight(node);
    updateHeight(child);
    // 返回旋转后子树的根结点
    return child;
}

```

### 左旋

!!! question "左旋"

    **如果失衡结点的子树度为1**

    ![](https://p.ipic.vip/z2flay.jpg)

    **如果失衡结点的子树度为2**

    ![](https://p.ipic.vip/v9zlmw.jpg)

    观察发现，**「左旋」和「右旋」操作是镜像对称的，两者对应解决的两种失衡情况也是对称的。**根据对称性，我们可以很方便地从「右旋」推导出「左旋」。具体地，只需将「右旋」代码中的把所有的 left 替换为 right 、所有的 right 替换为 left ，即可得到「左旋」代码。

```java
/* 左旋操作 */
TreeNode leftRotate(TreeNode node) {
    TreeNode child = node.right;
    TreeNode grandChild = child.left;
    // 以 child 为原点，将 node 向左旋转
    child.left = node;
    node.right = grandChild;
    // 更新结点高度
    updateHeight(node);
    updateHeight(child);
    // 返回旋转后子树的根结点
    return child;
}

```


### 先左后右

!!! question "先左后右"

    **单一使用左旋或右旋都无法使子树恢复平衡，此时需要「先左旋后右旋」**

    ![](https://p.ipic.vip/fyhd04.jpg)

### 先右后左

!!! question "先右后左"

    **「先右旋后左旋」**

    ![](https://p.ipic.vip/1el9po.jpg)

### 旋转的选择

!!! question "旋转的选择"

    ![](https://p.ipic.vip/35j3wa.jpg)

    - 失衡结点的平衡因子：**node**
    - 子结点的平衡因子：**child**

    <div class="center-table" markdown>

    | 失衡结点的平衡因子 | 子结点的平衡因子 | 应采用的旋转方法 |
    | ------------------ | ---------------- | ---------------- |
    | $>0$ （即左偏树）  | $\geq 0$         | 右旋             |
    | $>0$ （即左偏树）  | $<0$             | 先左旋后右旋     |
    | $<0$ （即右偏树）  | $\leq 0$         | 左旋             |
    | $<0$ （即右偏树）  | $>0$             | 先右旋后左旋     |

    </div>

```java
/* 执行旋转操作，使该子树重新恢复平衡 */
TreeNode rotate(TreeNode node) {
    // 获取结点 node 的平衡因子
    int balanceFactor = balanceFactor(node);
    // 左偏树
    if (balanceFactor > 1) {
        if (balanceFactor(node.left) >= 0) {
            // 右旋
            return rightRotate(node);
        } else {
            // 先左旋后右旋
            node.left = leftRotate(node.left);
            return rightRotate(node);
        }
    }
    // 右偏树
    if (balanceFactor < -1) {
        if (balanceFactor(node.right) <= 0) {
            // 左旋
            return leftRotate(node);
        } else {
            // 先右旋后左旋
            node.right = rightRotate(node.right);
            return leftRotate(node);
        }
    }
    // 平衡树，无需旋转，直接返回
    return node;
}

```

## 常用操作

### 插入结点

插入结点后，从该结点到根结点的路径上会出现一系列「失衡结点」。所以，**需要从该结点开始，从底至顶地执行旋转操作，使所有失衡结点恢复平衡。**

!!! Quote "通过递归找到符合插入条件的结点，创建`new TreeNode(val)`后开始回溯，回溯过程途径的结点会更新结点高度，如果平衡因子出现问题会进行旋转，直到回溯到根结点为止，此时整个树恢复平衡。"

    ```java
    /* 插入结点 */
    TreeNode insert(int val) {
        root = insertHelper(root, val);
        return root;
    }

    /* 递归插入结点（辅助方法） */
    TreeNode insertHelper(TreeNode node, int val) {
        if (node == null) return new TreeNode(val);
        /* 1. 查找插入位置，并插入结点 */
        if (val < node.val)
            node.left = insertHelper(node.left, val);
        else if (val > node.val)
            node.right = insertHelper(node.right, val);
        else
            return node;     // 重复结点不插入，直接返回
        updateHeight(node);  // 更新结点高度
        /* 2. 执行旋转操作，使该子树重新恢复平衡 */
        node = rotate(node);
        // 返回子树的根结点
        return node;
    }


    ```

### 删除结点

!!! Quote "「AVL 树」删除结点操作与「二叉搜索树」删除结点操作总体相同。类似地，在删除结点后，也需要从底至顶地执行旋转操作，使所有失衡结点恢复平衡。"

    ```java
    /* 删除结点 */
    TreeNode remove(int val) {
        root = removeHelper(root, val);
        return root;
    }

    /* 递归删除结点（辅助方法） */
    TreeNode removeHelper(TreeNode node, int val) {
        if (node == null) return null;
        /* 1. 查找结点，并删除之 */
        if (val < node.val)
        node.left = removeHelper(node.left, val);
        else if (val > node.val)
        node.right = removeHelper(node.right, val);
        else {
            if (node.left == null || node.right == null) {
                TreeNode child = node.left != null ? node.left : node.right;
                // 子结点数量 = 0 ，直接删除 node 并返回
                if (child == null)
                return null;
                // 子结点数量 = 1 ，直接删除 node
                else
                node = child;
            } else {
                // 子结点数量 = 2 ，则将中序遍历的下个结点删除，并用该结点替换当前结点
                TreeNode temp = getInOrderNext(node.right);
                node.right = removeHelper(node.right, temp.val);
                node.val = temp.val;
            }
        }
        updateHeight(node);  // 更新结点高度
        /* 2. 执行旋转操作，使该子树重新恢复平衡 */
        node = rotate(node);
        // 返回子树的根结点
        return node;
    }

    /* 获取中序遍历中的下一个结点（仅适用于 root 有左子结点的情况） */
    TreeNode getInOrderNext(TreeNode node) {
        if (node == null) return node;
        // 循环访问左子结点，直到叶结点时为最小结点，跳出
        while (node.left != null) {
            node = node.left;
        }
        return node;
    }

    ```

### 查找结点

!!! Quote "与二叉搜索树一致"

    ```java
    /* 查找结点 */
    TreeNode search(int num) {
        TreeNode cur = root;
        // 循环查找，越过叶结点后跳出
        while (cur != null) {
            // 目标结点在 cur 的右子树中
            if (cur.val < num) cur = cur.right;
            // 目标结点在 cur 的左子树中
            else if (cur.val > num) cur = cur.left;
            // 找到目标结点，跳出循环
            else break;
        }
        // 返回目标结点
        return cur;
    }

    ```

## 常见应用

- 组织存储大型数据，适用于高频查找、低频增删场景；
- 用于建立数据库中的索引系统；

!!! question "为什么红黑树比 AVL 树更受欢迎？"

    红黑树的平衡条件相对宽松，因此在红黑树中插入与删除结点所需的旋转操作相对更少，结点增删操作相比 AVL 树的效率更高。

## `leetcode`

!!! example "1382. 将二叉搜索树变平衡"

    给你一棵二叉搜索树，请你返回一棵 平衡后 的二叉搜索树，新生成的树应该与原来的树有着相同的节点值。如果有多种构造方法，请你返回任意一种。

    如果一棵二叉搜索树中，每个节点的两棵子树高度差不超过 `1` ，我们就称这棵二叉搜索树是 **平衡的** 。

    本地最优解是先用中序遍历把二叉树转换为有序数组，然后构造平衡二叉搜索树，不过为了熟悉旋转，本题使用手撕方式。

    ```java
    class Solution {
        public TreeNode balanceBST(TreeNode root) {
            if (root == null){
                return null;
            }
            // node节点的高度缓存
            Map<TreeNode,Integer> nodeHeight = new HashMap<>();
            TreeNode newRoot = null;
            Deque<TreeNode> stack = new LinkedList<>();
                TreeNode node = root;
                // 先序遍历插入（其实用哪个遍历都行）
                while(node != null || !stack.isEmpty()){
                    if (node != null){
                        // 新树插入
                        newRoot = insert(newRoot,node.val,nodeHeight);
                        stack.push(node);
                        node = node.left;
                    }else {
                        node = stack.pop();
                        node = node.right;
                    }
                }
                return newRoot;
            }

            /**
            * 新节点插入
            * @param root root
            * @param val 新加入的值
            * @param nodeHeight 节点高度缓存
            * @return 新的root节点
            */
            private TreeNode insert(TreeNode root,int val,Map<TreeNode,Integer> nodeHeight){
                if (root == null){
                    root = new TreeNode(val);
                    nodeHeight.put(root,1);// 新节点的高度
                    return root;
                }
                TreeNode node = root;
                int cmp = val - node.val;
                if (cmp < 0){
                    // 左子树插入
                    node.left = insert(root.left,val,nodeHeight);
                    // 如果左右子树高度差超过1，进行旋转调整
                    if (nodeHeight.getOrDefault(node.left,0) - nodeHeight.getOrDefault(node.right,0) > 1){
                        if (val > node.left.val){
                            // 插入在左孩子右边，左孩子先左旋
                            node.left = rotateLeft(node.left,nodeHeight);
                        }
                        // 节点右旋
                        node = rotateRight(node,nodeHeight);
                    }
                }else if (cmp > 0){
                    // 右子树插入
                    node.right = insert(root.right,val,nodeHeight);
                    // 如果左右子树高度差超过1，进行旋转调整
                    if (nodeHeight.getOrDefault(node.right,0) - nodeHeight.getOrDefault(node.left,0) > 1){
                        if (val < node.right.val){
                            // 插入在右孩子左边，右孩子先右旋
                            node.right = rotateRight(node.right,nodeHeight);
                        }
                        // 节点左旋
                        node = rotateLeft(node,nodeHeight);
                    }
                }else {
                    // 一样的节点，啥都没发生
                    return node;
                }
                // 获取当前节点新高度
                int height =  getCurNodeNewHeight(node,nodeHeight);
                // 更新当前节点高度
                nodeHeight.put(node,height);
                return node;
            }

            /**
            * node节点左旋
            * @param node node
            * @param nodeHeight node高度缓存
            * @return 旋转后的当前节点
            */
            private TreeNode rotateLeft(TreeNode node,Map<TreeNode,Integer> nodeHeight){
                // ---指针调整
                TreeNode right = node.right;
                node.right = right.left;
                right.left = node;
                // ---高度更新
                // 先更新node节点的高度，这个时候node是right节点的左孩子
                int newNodeHeight = getCurNodeNewHeight(node,nodeHeight);
                // 更新node节点高度
                nodeHeight.put(node,newNodeHeight);
                // newNodeHeight是现在right节点左子树高度，原理一样，取现在right左右子树最大高度+1
                int newRightHeight = Math.max(newNodeHeight,nodeHeight.getOrDefault(right.right,0)) + 1;
                // 更新原right节点高度
                nodeHeight.put(right,newRightHeight);
                return right;
            }

            /**
            * node节点右旋
            * @param node node
            * @param nodeHeight node高度缓存
            * @return 旋转后的当前节点
            */
            private TreeNode rotateRight(TreeNode node,Map<TreeNode,Integer> nodeHeight){
                // ---指针调整
                TreeNode left = node.left;
                node.left = left.right;
                left.right = node;
                // ---高度更新
                // 先更新node节点的高度，这个时候node是right节点的左孩子
                int newNodeHeight = getCurNodeNewHeight(node,nodeHeight);
                // 更新node节点高度
                nodeHeight.put(node,newNodeHeight);
                // newNodeHeight是现在left节点右子树高度，原理一样，取现在right左右子树最大高度+1
                int newLeftHeight = Math.max(newNodeHeight,nodeHeight.getOrDefault(left.left,0)) + 1;
                // 更新原left节点高度
                nodeHeight.put(left,newLeftHeight);
                return left;
            }

            /**
            * 获取当前节点的新高度
            * @param node node
            * @param nodeHeight node高度缓存
            * @return 当前node的新高度
            */
            private int getCurNodeNewHeight(TreeNode node,Map<TreeNode,Integer> nodeHeight){
                // node节点的高度，为现在node左右子树最大高度+1
                return Math.max(nodeHeight.getOrDefault(node.left,0),nodeHeight.getOrDefault(node.right,0)) + 1;
            }
        }

    ```

