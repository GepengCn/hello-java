# 队列`Queue`
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