# 栈
2023-03-21

61

## 第一次测试
特性 18 6

- 先入后出
- 线性

## java用法 12 0

实现方式 40

链表 20 10

```java
public class Stack<Item>{
    public Node<Item> first;
    public int N;

    public class Node{
        public Item item;
        public Node next;
    }

    public boolean isEmpty(){return first == null;}

    public int size(){return N;}

    public void push(Item item){
        Node oldFirst = first;
        //first = new Node();
        first.item = item;
        first.next = oldFirst;
        N++;
    }

    public Item pop(){

        Item item = first.item;

        first = first.next;

        N--;

        return item;
    }

}
```


数组 20 20

```java
public class Stack<Item>{

    private ArrayList<Item> list;

    public boolean isEmpty(){return list.size() == 0;}

    public int size(){return list.size();}

    public void push(Item item){
        list.add(item);
    }

    public Item pop(){
        return list.remove(list.size()-1);
    }
}

```

应用 30 25

1. 平衡符号
编译器编译括号的合法性，是否闭环，利用一个栈来实现。
读取文件直到尾部，如果是起始括号，则推入栈中。如果是结尾括号，判断栈是否为空。如果栈为空，则报错；如果不为空，则推出栈顶元素。如果推出的元素与当前括号不匹配，则报错。
如果扫描到尾部后，栈不为空，则报错。
2. 浏览器的后退与前进，软件的撤销与反撤销。
3. 方法调用，每当有新方法调用，则将函数变量、地址等推入栈顶，该引用为栈帧，方法执行完后，执行出栈，即栈帧出栈或栈顶元素出栈。函数递归