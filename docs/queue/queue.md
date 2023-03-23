# 队列`Queue`
## 思维导图
![](https://p.ipic.vip/bypa82.jpg){width=500}

## 特性

- 先入先出
- 线性
- 队首：队列头部
- 队尾：队列尾部
- 入队：加入队尾
- 出队：删除队首

## `Java`用法
```java
Queue<Integer> queue = new LinkedList<>();
queue.offer(1);//入队
int peek = queue.peek();//访问队首元素
int pop = queue.pop();//出队
int size = queue.size();//获取队列的长度
boolean isEmpty = queue.isEmpty();//判断队列是否为空
```

## 实现方式

!!! tip "链表"

    使用两个单链表，一个作为队首，一个作为队尾

```java
public class LinkedListQueue{
    private ListNode front, rear;
    private int N;

    public LinkedListQueue(){
        front = null;
        rear = null;
    }

    public int size(){return N;}

    public boolean isEmpty(){return size() == 0;}

    public void push(int num){
        ListNode node = new ListNode(num);
        if(front == null){
            front = node;
            rear = node;
        } else {
            rear.next = node;
            rear = node;
        }
        N++;
    }

    public int pop(){
        int peek = peek();
        front = front.next;
        N--;
        return peek;
    }

    public int peek(){
        if(size() ==0) throw new EmptyQueueException();
        return front.val;
    }

}
```

!!! tip "数组"

    环形数组

```java
public class ArrayQueue{
    private int[] nums;
    private int front;
    private int N;
    public ArrayQueue(int capacity){
        nums = new int[capacity];
        front = 0;
        N = 0;
    }

    public int capacity(){
        return nums.length;
    }

    public int size(){
        return N;
    }

    public boolean isEmpty(){
        return size() == 0;
    }

    public void push(int num){
        if(N == capacity()){
            throw new QueueCapacityException();
        }

        int rear = (front + N) % capacity();

        nums[rear] = num;

        N++;
    }

    public int peek(){
        if(size() ==0) throw new EmptyQueueException();
        return nums[front];
    }

    public int pop(){
        int peek = peek();
        front = (front+1) % capacity();
        N--;
        return peek;
    }
}
```

## 实际应用

!!! Note "淘宝订单"

    购物者下单后，订单就被加入到队列之中，随后系统再根据顺序依次处理队列中的订单

!!! Note "打印机的任务队列"

!!! Note "餐厅的出餐队列"

## `leetcode`

!!! example "225. 用队列实现栈"

    请你仅使用两个队列实现一个后入先出（LIFO）的栈，并支持普通栈的全部四种操作（push、top、pop 和 empty）。

    实现 MyStack 类：

    - void push(int x) 将元素 x 压入栈顶。
    - int pop() 移除并返回栈顶元素。
    - int top() 返回栈顶元素。
    - boolean empty() 如果栈是空的，返回 true ；否则，返回 false 。

    ```java
    class MyStack {

        private Queue<Integer> A;
        private Queue<Integer> B;

        public MyStack() {
            A = new LinkedList<>();
            B = new LinkedList<>();
        }

        public void push(int x) {
            B.offer(x);
            while(!A.isEmpty())B.offer(A.poll());
            Queue<Integer> temp = A;
                A = B;
                B = temp;
            }

            public int pop() {
                return A.poll();
            }

            public int top() {
                return A.peek();
            }

            public boolean empty() {
                return A.size() == 0;
            }
        }

    ```

!!! example "232. 用栈实现队列"

    请你仅使用两个栈实现先入先出队列。队列应当支持一般队列支持的所有操作（push、pop、peek、empty）：

    实现 MyQueue 类：

    - void push(int x) 将元素 x 推到队列的末尾
    - int pop() 从队列的开头移除并返回元素
    - int peek() 返回队列开头的元素
    - boolean empty() 如果队列为空，返回 true ；否则，返回 false

    ```java
    class MyQueue {

        Stack<Integer> A;

        Stack<Integer> B;

        public MyQueue() {
            A = new Stack<>();
            B = new Stack<>();
        }

        public void push(int x) {
            B.push(x);

        }

        public int pop() {

            if(A.isEmpty()){
                while(!B.isEmpty())A.push(B.pop());
            }

            return A.pop();
        }

        public int peek() {
            if(A.isEmpty()){
                while(!B.isEmpty())A.push(B.pop());
            }

            return A.peek();
        }

        public boolean empty() {
            return A.isEmpty()&&B.isEmpty();
        }
    }
    ```

!!! example "剑指 Offer 50. 第一个只出现一次的字符"

    在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。

    ```java
    class Solution {
        public char firstUniqChar(String s) {

        Map<Character, Integer> dict = new HashMap<>();

        Queue<Character> queue = new LinkedList<>();

            for(int i=0;i<s.length();i++){
                char c = s.charAt(i);
                if(!dict.containsKey(c)){
                    dict.put(c, i);
                    queue.offer(c);
                } else {
                    dict.put(c,-1);
                    while(!queue.isEmpty()&&dict.get(queue.peek())==-1)queue.poll();

                }

            }

            return queue.isEmpty() ? ' ' : queue.poll();
        }
    }
    ```