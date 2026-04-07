# 超详细、零基础入门：数据结构与算法——**队列（Queue）**
我给你讲得**全面、通俗、一步到位**，不跳步、不玄学，适合大二计算机专业直接用来复习/写作业/备考。

---

# 一、队列是什么？（最本质定义）
**队列是一种「先进先出」的线性数据结构**
- FIFO：First In First Out
- 就像：**排队买票、食堂打饭、打印机任务、消息队列**

特点：
1. 只能在**队尾（rear）插入**
2. 只能在**队头（front）删除**
3. 中间不能随便删、不能插队

---

# 二、队列的核心操作
所有队列都必须支持这 5 个基础操作：

1. **enqueue(x)**：入队 → 加到队尾
2. **dequeue()**：出队 → 删除队头并返回
3. **front()**：查看队头元素
4. **isEmpty()**：判断是否为空
5. **isFull()**：判断是否满（顺序队列需要）

---

# 三、队列的两种实现方式
## 1. 顺序队列（数组实现）
用一段连续内存（数组）存元素。
问题：
- 普通顺序队列会出现**“假溢出”**
- 前面空了，后面满了，数组却用不了

所以实际开发中**几乎不用普通顺序队列**，都用：

## 2. 循环队列（最常用！面试必考）
把数组**首尾相连**，解决假溢出。

关键公式：
- 队空：`front == rear`
- 队满：`(rear + 1) % maxSize == front`
- 入队：`rear = (rear + 1) % maxSize`
- 出队：`front = (front + 1) % maxSize`
- 元素个数：`(rear - front + maxSize) % maxSize`

---

## 3. 链式队列（链表实现）
- 没有容量限制
- 不会假溢出
- 适合**不确定长度**的场景

结构：
- 头指针 front：指向队头结点
- 尾指针 rear：指向队尾结点

入队：尾插
出队：头删

---

# 四、队列 vs 栈（对比记忆，考试最爱考）

| 特性         | 栈 Stack           | 队列 Queue         |
|------------|-------------------|-------------------|
| 进出原则       | 后进先出 LIFO       | 先进先出 FIFO       |
| 操作端        | 只在栈顶操作         | 队头删、队尾插       |
| 典型应用       | 函数调用、括号匹配、逆序 | 排队、消息、BFS、缓冲 |
| 实现         | 数组/链表          | 循环数组/链表       |

---

# 五、队列的分类（非常重要）
## 1. 普通队列
最基础，严格先进先出。

## 2. 双端队列 Deque
Double-Ended Queue
- 两头都能进、两头都能出
- C++ `deque`、Java `Deque`、Python `deque` 都是这个

## 3. 优先队列 Priority Queue
不是按进入顺序，而是**按优先级出队**
底层通常用**堆（heap）**实现
常用于：Dijkstra、Huffman编码、任务调度

## 4. 阻塞队列 / 并发队列
多线程用：
- 满了就阻塞入队
- 空了就阻塞出队
用于：生产者消费者模型

---

# 六、队列的经典应用（面试 & 作业高频）
1. **BFS 广度优先搜索**（图、树的层序遍历）
2. 操作系统 **进程调度**
3. **打印机任务队列**
4. **消息队列 MQ**（Kafka、RabbitMQ、Redis队列）
5. 浏览器 **前进后退**（双端队列）
6. 缓冲区、流量控制
7. 并发任务排队

---

# 七、C++ 手写循环队列（可直接交作业）
```cpp
#include <iostream>
using namespace std;

const int MAXSIZE = 100;

class CircularQueue {
private:
    int data[MAXSIZE];
    int front, rear;

public:
    CircularQueue() {
        front = rear = 0;
    }

    // 判空
    bool isEmpty() {
        return front == rear;
    }

    // 判满
    bool isFull() {
        return (rear + 1) % MAXSIZE == front;
    }

    // 入队
    bool enqueue(int x) {
        if (isFull()) return false;
        data[rear] = x;
        rear = (rear + 1) % MAXSIZE;
        return true;
    }

    // 出队
    bool dequeue(int &x) {
        if (isEmpty()) return false;
        x = data[front];
        front = (front + 1) % MAXSIZE;
        return true;
    }

    // 取队头
    bool getFront(int &x) {
        if (isEmpty()) return false;
        x = data[front];
        return true;
    }
};
```

---

# 八、notice

1. **什么是假溢出？怎么解决？**
   数组前面空、后面满，叫假溢出。用**循环队列**解决。

2. **循环队列为什么要空一个位置？**
   为了区分**队空**和**队满**。
   不空的话 `front == rear` 既表示空又表示满，无法判断。

3. **队列和广度优先搜索 BFS 有什么关系？**
   BFS 就是**用队列实现**的：
   每次访问一层，把下一层全部入队，保证“先访问先扩展”。

4. **优先队列是队列吗？**
   逻辑上是，但**底层不是普通队列**，是堆。   具体请看堆篇章以及树篇章

---
