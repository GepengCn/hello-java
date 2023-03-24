# 二叉树`binary_tree`

## 思维导图

![](https://p.ipic.vip/dcmg3l.jpg)

## 特性

- 非线性
- 一分为二的`分治逻辑`
- 节点包含一个值和两个指针
- 根节点：二叉树最顶层的节点，其没有父节点
- 叶节点：没有子节点的节点，两个指针都指向`null`
- 层：从顶至低递增，根节点为1
- 度：节点的子节点数量，二叉树中，度的范围0，1，2
- 边：连接两个节点的边，即指针
- 二叉树高度：根节点到最远叶节点经过的边的数量
- 节点深度：根节点到此节点经过的边的数量
- 节点高度：此节点到最远叶节点经过的边的数量

## 常用操作

!!! Quote "初始化"

    ```java
    TreeNode n1 = new TreeNode(1);
    TreeNode n2 = new TreeNode(2);
    TreeNode n3 = new TreeNode(3);
    ```
!!! Quote "构建引用"

    ```java
    n1.left = n2;
    n1.right = n3;
    n2.left = n4;
    n2.right = n5;
    ```

!!! Quote "插入与删除"

    ```java
    ```java
    TreeNode P = new TreeNode(0);
    // 在 n1 -> n2 中间插入结点 P
    n1.left = P;
    P.left = n2;
    // 删除结点 P
    n1.left = n2;
    ```
    ![](https://p.ipic.vip/4emu04.jpg)

## 常见二叉树类型

!!! Info "完美二叉树"

    - 也叫满二叉树，所有层的结点都被完全填满。
    - 所有节点的度为2
    - 若树高度 $= h$ ，则结点总数 $= 2^{h+1} - 1$
    - `Perfect Binary Tree`

    ![](https://p.ipic.vip/f4ow0b.jpg)

!!! Info "完全二叉树"

    - 最底层的结点未被填满，且最底层结点尽量靠左填充
    - **完全二叉树非常适合用数组来表示**:如果按照层序遍历序列的顺序来存储，那么空结点 null 一定全部出现在序列的尾部，因此我们就可以不用存储这些 null 了。

    ![](https://p.ipic.vip/d0296p.jpg)

!!! Info "完满二叉树"

    - 叶结点之外，其余所有结点都有两个子结点

    ![](https://p.ipic.vip/c5z563.jpg)

!!! Info "平衡二叉树"

    - 任意结点的左子树和右子树的高度之差的绝对值 $\leq 1$

    ![](https://p.ipic.vip/329z8e.jpg)

## 二叉树的退化

当二叉树的每层的结点都被填满时，达到「完美二叉树」；而当所有结点都偏向一边时，二叉树退化为「链表」。

- 完美二叉树是一个二叉树的“最佳状态”，可以完全发挥出二叉树“分治”的优势；
- 链表则是另一个极端，各项操作都变为线性操作，时间复杂度退化至 $O(n)$；

![](https://p.ipic.vip/2rfi23.jpg)

## 遍历方式

![](https://p.ipic.vip/q5fjpy.jpg)

- 前序遍历（先根，再左，最后右）
- 中序遍历（先左，再根，最后右）
- 后序遍历（先左，再右，最后根）

## 中序遍历算法实现

!!! Tip "广度优先搜索 BFS"

    ```java

    List<Integer> levelOrder(TreeNode root) {
    // 初始化队列，加入根结点
    Queue<TreeNode> queue = new LinkedList<>() {{ add(root); }};
    // 初始化一个列表，用于保存遍历序列
    List<Integer> list = new ArrayList<>();
        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();  // 队列出队
            list.add(node.val);            // 保存结点值
            if (node.left != null)
            queue.offer(node.left);    // 左子结点入队
            if (node.right != null)
            queue.offer(node.right);   // 右子结点入队
        }
        return list;
    }

    ```

!!! Tip "深度优先搜索 DFS"

    ```java
    class Solution {
        public List<Integer> inorderTraversal(TreeNode root) {
            List<Integer> res = new ArrayList<Integer>();
            inorder(root, res);
            return res;
        }

        public void inorder(TreeNode root, List<Integer> res) {
            if (root == null) {
                return;
            }
            inorder(root.left, res);
            res.add(root.val);
            inorder(root.right, res);
        }
    }
    ```

## `leetcode`


### 剑指 Offer 32 - I. 从上到下打印二叉树

!!! example "剑指 Offer 32 - I. 从上到下打印二叉树"

    从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。

    ```
    3
    / \
    9  20
    /  \
    15   7
    ```

    返回：

    ```
    [3,9,20,15,7]
    ```

    ```java
    class Solution {
        public int[] levelOrder(TreeNode root) {
            if(root == null) return new int[0];
            Queue<TreeNode> queue = new LinkedList<>(){{ add(root); }};
            ArrayList<Integer> ans = new ArrayList<>();
            while(!queue.isEmpty()) {
                TreeNode node = queue.poll();
                ans.add(node.val);
                if(node.left != null) queue.add(node.left);
                if(node.right != null) queue.add(node.right);
            }
            int[] res = new int[ans.size()];
            for(int i = 0; i < ans.size(); i++)
            res[i] = ans.get(i);
            return res;
        }
    }

    ```



### 剑指 Offer 32 - II. 从上到下打印二叉树 II

!!! example "剑指 Offer 32 - II. 从上到下打印二叉树 II"

        从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。

        例如:
        给定二叉树: [3,9,20,null,null,15,7],

        ```
        3
        / \
        9  20
        /  \
        15   7
        ```
        返回其层次遍历结果：

        ```
        [
            [3],
            [9,20],
            [15,7]
        ]
        ```

        ```java
        class Solution {
            public List<List<Integer>> levelOrder(TreeNode root) {
                Queue<TreeNode> queue = new LinkedList<>();
                List<List<Integer>> res = new ArrayList<>();
                if(root != null) queue.add(root);
                while(!queue.isEmpty()) {
                List<Integer> tmp = new ArrayList<>();
                    for(int i = queue.size(); i > 0; i--) {
                        TreeNode node = queue.poll();
                        tmp.add(node.val);
                        if(node.left != null) queue.add(node.left);
                        if(node.right != null) queue.add(node.right);
                    }
                    res.add(tmp);
                }
                return res;
            }
        }

        ```

### 剑指 Offer 32 - III. 从上到下打印二叉树 III

!!! example "剑指 Offer 32 - III. 从上到下打印二叉树 III"

    请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。

    例如:
    给定二叉树: [3,9,20,null,null,15,7],

    ```
    3
    / \
    9  20
    /  \
    15   7
    ```
    返回其层次遍历结果：
    ```
    [
        [3],
        [20,9],
        [15,7]
    ]
    ```

    ```java
    class Solution {
        public List<List<Integer>> levelOrder(TreeNode root) {
            Queue<TreeNode> queue = new LinkedList<>();
            List<List<Integer>> res = new ArrayList<>();
            if(root != null) queue.add(root);
            while(!queue.isEmpty()) {
            List<Integer> tmp = new ArrayList<>();
                for(int i = queue.size(); i > 0; i--) {
                    TreeNode node = queue.poll();
                    tmp.add(node.val);
                    if(node.left != null) queue.add(node.left);
                    if(node.right != null) queue.add(node.right);
                }
                if(res.size() % 2 == 1) Collections.reverse(tmp);
                res.add(tmp);
            }
            return res;
        }
    }

    ```

### 剑指 Offer 27. 二叉树的镜像

!!! example "剑指 Offer 27. 二叉树的镜像"

    请完成一个函数，输入一个二叉树，该函数输出它的镜像。

    例如输入：

    ```
          4
        /   \
       2     7
      / \   / \
     1   3 6   9
    ```
    镜像输出：

    ```
          4
        /   \
       7     2
      / \   / \
     9   6 3   1
    ```

    ```java
    class Solution {
        public TreeNode mirrorTree(TreeNode root) {
            if(root == null) return null;
            TreeNode tmp = root.left;
            root.left = mirrorTree(root.right);
            root.right = mirrorTree(tmp);
            return root;
        }
    }

    ```
