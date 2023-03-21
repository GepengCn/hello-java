# 栈 `Stack`

## 特性
- 先入后出
- 线性
- 类比与桌子上的邮件，新邮件放在最上面
- 栈元素的顶部俗称栈顶
- 入栈：元素添加到栈顶
- 出栈：删除栈顶元素

## `Java`用法
```java
Stack<Integer> stack = new Stack<>();
stack.push(1);//入栈
int pop = stack.pop();//出栈
int peek = stack.peek();//访问栈顶元素，不删除栈顶元素
```

## 实现方式

!!! tip "1. 链表"

    使用单链表，链表顶端插入实现push， 删除顶端实现pop，访问顶端实现peek。

```java
public class Stack<Item> {

    private Node first;

    private int N;

    private class Node {
        Item item;
        Node next;
    }

    public boolean isEmpty(){return first == null;}

    public int size(){return N;}

    public void push(Item item){
        Node oldFirst = first;
        first = new Node();
        first.item = item;
        first.next = oldFirst;
        N++;
    }

    public Item pop(){
        Item item = first.item;
        first = first.next;
        N--;
    }
}

```

!!! tip "2. 数组"

    使用数组，尾部插入元素实现push，尾部删除元素实现pop

```java
public class Stack<Item> {

    private ArrayList<Item> list;

    public boolean isEmpty(){return list.size()==0;}

    public int size(){return list.size();}

    public void push(Item item){
        list.add(item);
    }

    public Item pop(){
        return list.remove(list.size()-1);
    }
}

```

## 实际应用

!!! Note "平衡符号"

    编译器检查花括号、方括号、圆括号等是否成对出现，可以使用栈来实现。做一个空栈。读入字符直到文件末尾。a.遇到起始括号，则推入栈；b.遇到结尾括号，如果栈为空则报错。否则，将栈顶元素弹出，如果弹出的元素不是对应的起始括号，则报错。如果读到文件末尾时，栈不为空，也报错。

!!! Note "方法调用"

    每当调用函数时，系统就会在栈顶添加一个栈帧，用来记录函数的上下文信息。在递归函数中，向下递推会不断执行入栈，向上回溯阶段时出栈。

!!! Note "浏览器前进后退, 软件中的撤销与反撤销"

    每当我们打开新的网页，浏览器就将上一个网页执行入栈，这样我们就可以通过「后退」操作来回到上一页面，后退操作实际上是在执行出栈。如果要同时支持后退和前进，那么则需要两个栈来配合实现。

## `leetcode`

!!! example "20. 有效的括号"

    给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。

    有效字符串需满足：

    1. 左括号必须用相同类型的右括号闭合。

    2. 左括号必须以正确的顺序闭合。

    3. 每个右括号都有一个对应的相同类型的左括号。
    ```java
    class Solution {
        public boolean isValid(String s) {
            Stack<Character> stack = new Stack<>();
                Map<Character, Character> dict = new HashMap<>();

                dict.put(')','(');
                dict.put(']','[');
                dict.put('}','{');

                for(char c: s.toCharArray()){

                    if(c == '(' || c == '[' || c == '{'){
                        stack.push(c);
                    }else {
                        if(stack.isEmpty())return false;

                        if(stack.pop() != dict.get(c))return false;
                    }
                }
                return stack.isEmpty();
            }
        }
    ```
    `ASCII码表`中起始括号与结尾括号相差1或者2，所以以上代码还可以优化
    ```java
    class Solution {
        public boolean isValid(String s) {
            Stack<Character> stack = new Stack<>();

                for(char c: s.toCharArray()){

                    if(c == '(' || c == '[' || c == '{'){
                        stack.push(c);
                    }else {
                        if(stack.isEmpty())return false;
                        int pop = stack.pop();
                        if(pop != c-1 && pop != c-2)return false;
                    }
                }
                return stack.isEmpty();
            }
        }
    ```

!!! example "234. 回文链表"

    给你一个单链表的头节点 head ，请你判断该链表是否为回文链表。如果是，返回 true ；否则，返回 false 。