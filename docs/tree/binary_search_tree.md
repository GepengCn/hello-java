# 二叉搜索树`binary_search_tree`

## 思维导图

![](https://p.ipic.vip/otn4eg.jpg)

## 特性


- 对于任一根结点， 左子树的值 $<$ 根结点的值 $<$ 右子树的值
- 使用中序遍历可以按`序`输出结点
- 查询、插入与删除的时间复杂度为$O(\log n)$
- 动态地在二叉搜索树中插入与删除结点，则可能导致二叉树退化为链表，此时各种操作的时间复杂度也退化为$O(N)$


## 常用操作

### 查询结点

!!! Quote "查询结点"

    由二叉搜索树的性质可以推论到，假设根结点为`node`，值为$n$，目标结点为$x$

    - $x$ $==$ $n$。**找到**结点
    - $x$ $<$ $n$。推测目标值应该在**左节点继续找**。
    - $x$ $>$ $n$。推测目标值应该在**右节点继续找**。
    - 此方法类似于二分查找
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

### 插入结点

!!! Quote "插入结点"

    给定一个待插入元素$n$，为了保持 **左子树 $<$ 根结点 $<$ 右子树**的性质，分两步操作

    - 找到对应结点的位置
    - 初始化该结点，并插入改位置（通过更改对应根节点的指针指向）

    ```java
    /* 插入结点 */
    TreeNode insert(int num) {
        // 若树为空，直接提前返回
        if (root == null) return null;
        TreeNode cur = root, pre = null;
        // 循环查找，越过叶结点后跳出
        while (cur != null) {
            // 找到重复结点，直接返回
            if (cur.val == num) return null;
            pre = cur;
            // 插入位置在 cur 的右子树中
            if (cur.val < num) cur = cur.right;
            // 插入位置在 cur 的左子树中
            else cur = cur.left;
        }
        // 插入结点 val
        TreeNode node = new TreeNode(num);
        if (pre.val < num) pre.right = node;
        else pre.left = node;
        return node;
    }

    ```
### 删除结点

!!! Quote "删除结点"

    同插入结点，需保持 **左子树 $<$ 根结点 $<$ 右子树**的性质， 先通过查找获取待删除的结点，通过子结点数量删除分以下三种情况：

    - **子结点数量$=0$**：表明是叶子结点，直接删除即可
    - **子结点数量$=1$**：用**子结点**替换待删除结点
    - **子结点数量$=2$**：通过**中序遍历**查找待删除结点的下一结点，替换待删除结点

    ```java
    /* 删除结点 */
    TreeNode remove(int num) {
        // 若树为空，直接提前返回
        if (root == null) return null;
        TreeNode cur = root, pre = null;
        // 循环查找，越过叶结点后跳出
        while (cur != null) {
            // 找到待删除结点，跳出循环
            if (cur.val == num) break;
            pre = cur;
            // 待删除结点在 cur 的右子树中
            if (cur.val < num) cur = cur.right;
            // 待删除结点在 cur 的左子树中
            else cur = cur.left;
        }
        // 若无待删除结点，则直接返回
        if (cur == null) return null;
        // 子结点数量 = 0 or 1
        if (cur.left == null || cur.right == null) {
            // 当子结点数量 = 0 / 1 时， child = null / 该子结点
            TreeNode child = cur.left != null ? cur.left : cur.right;
            // 删除结点 cur
            if (pre.left == cur) pre.left = child;
            else pre.right = child;
        }
        // 子结点数量 = 2
        else {
            // 获取中序遍历中 cur 的下一个结点
            TreeNode nex = getInOrderNext(cur.right);
            int tmp = nex.val;
            // 递归删除结点 nex
            remove(nex.val);
            // 将 nex 的值复制给 cur
            cur.val = tmp;
        }
        return cur;
    }

    /* 获取中序遍历中的下一个结点（仅适用于 root 有左子结点的情况） */
    TreeNode getInOrderNext(TreeNode root) {
        if (root == null) return root;
        // 循环访问左子结点，直到叶结点时为最小结点，跳出
        while (root.left != null) {
            root = root.left;
        }
        return root;
    }

    ```
### 升序遍历

!!! Quote "升序遍历"

    ```java
    void dfs(TreeNode root) {
        if(root == null) return;
        dfs(root.left); // 左
        System.out.println(root.val); // 根
        dfs(root.right); // 右
    }
    ```

### 降序遍历

!!! Quote "降序遍历"

    ```java
    void dfs(TreeNode root) {
        if(root == null) return;
        dfs(root.right); // 右
        System.out.println(root.val); // 根
        dfs(root.left); // 左
    }
    ```

## 常见应用

- 系统中的多级索引，高效查找、插入、删除操作。
- 各种搜索算法的底层数据结构。
- 存储数据流，保持其已排序。

## `leetcode`

### 剑指 Offer 54. 二叉搜索树的第k大节点

!!! example "剑指 Offer 54. 二叉搜索树的第k大节点"

    给定一棵二叉搜索树，请找出其中第 k 大的节点的值。

    ```java
    class Solution {
        int res, k;
        public int kthLargest(TreeNode root, int k) {
            this.k = k;
            dfs(root);
            return res;
        }
        void dfs(TreeNode root) {
            if(root == null) return;
            dfs(root.right);
            if(k == 0) return;
            if(--k == 0) res = root.val;
            dfs(root.left);
        }
    }

    ```

### 98. 验证二叉搜索树

!!! example "98. 验证二叉搜索树"

    给你一个二叉树的根节点 `root` ，判断其是否是一个有效的二叉搜索树。

    ```java
    class Solution {
        long pre = Long.MIN_VALUE;
        public boolean isValidBST(TreeNode root) {
            if (root == null) {
                return true;
            }
            // 访问左子树
            if (!isValidBST(root.left)) {
                return false;
            }
            // 访问当前节点：如果当前节点小于等于中序遍历的前一个节点，说明不满足BST，返回 false；否则继续遍历。
            if (root.val <= pre) {
                return false;
            }
            pre = root.val;
            // 访问右子树
            return isValidBST(root.right);
        }
    }

    ```