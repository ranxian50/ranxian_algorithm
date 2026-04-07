我给你写**最标准、最完整、可直接运行、适合作业/考试**的 C++ 队列实现，包含：

1. **循环队列（数组实现，面试必考）**
2. **链式队列（链表实现）**
3. **STL 队列用法（实际开发用）**

全部带**详细注释 + 测试主函数**，复制就能跑。

---

# 一、循环队列（数组实现）【最推荐掌握】
解决假溢出，工业界最常用的队列实现。

```cpp
#include <iostream>
using namespace std;

// 循环队列类
class CircularQueue {
private:
    int* arr;       // 数组
    int front;      // 队头指针
    int rear;       // 队尾指针
    int maxSize;    // 最大容量

public:
    // 构造函数：初始化队列
    CircularQueue(int size) {
        maxSize = size + 1; // 空出一个位置区分队空/队满
        arr = new int[maxSize];
        front = rear = 0;
    }

    // 析构函数：释放内存
    ~CircularQueue() {
        delete[] arr;
    }

    // 判断队列是否为空
    bool isEmpty() {
        return front == rear;
    }

    // 判断队列是否已满
    bool isFull() {
        return (rear + 1) % maxSize == front;
    }

    // 入队：成功返回true，失败false
    bool enQueue(int val) {
        if (isFull()) {
            cout << "队列已满，无法入队！" << endl;
            return false;
        }
        arr[rear] = val;
        rear = (rear + 1) % maxSize; // 循环移动
        return true;
    }

    // 出队：返回队头元素并删除
    bool deQueue() {
        if (isEmpty()) {
            cout << "队列为空，无法出队！" << endl;
            return false;
        }
        front = (front + 1) % maxSize;
        return true;
    }

    // 获取队头元素
    int getFront() {
        if (isEmpty()) {
            cout << "队列为空！" << endl;
            return -1;
        }
        return arr[front];
    }

    // 获取队列元素个数
    int size() {
        return (rear - front + maxSize) % maxSize;
    }

    // 遍历打印队列
    void printQueue() {
        if (isEmpty()) {
            cout << "队列为空！" << endl;
            return;
        }
        cout << "队列元素：";
        for (int i = front; i != rear; i = (i + 1) % maxSize) {
            cout << arr[i] << " ";
        }
        cout << endl;
    }
};

// 测试循环队列
int main() {
    CircularQueue q(5); // 创建容量为5的队列

    q.enQueue(10);
    q.enQueue(20);
    q.enQueue(30);
    q.printQueue(); // 10 20 30

    q.deQueue();
    q.printQueue(); // 20 30

    cout << "队头元素：" << q.getFront() << endl; // 20
    cout << "队列大小：" << q.size() << endl;     // 2

    q.enQueue(40);
    q.enQueue(50);
    q.enQueue(60);
    q.printQueue(); // 20 30 40 50 60

    q.enQueue(70); // 队列满

    return 0;
}
```

---

# 二、链式队列（链表实现）【无容量限制】
用单链表实现，不用考虑溢出，适合元素数量不确定的场景。

```cpp
#include <iostream>
using namespace std;

// 链表结点
struct Node {
    int data;
    Node* next;
    Node(int val) : data(val), next(nullptr) {}
};

// 链式队列类
class LinkQueue {
private:
    Node* front; // 队头
    Node* rear;  // 队尾
    int count;   // 元素个数

public:
    LinkQueue() {
        front = rear = nullptr;
        count = 0;
    }

    ~LinkQueue() {
        while (front != nullptr) {
            Node* temp = front;
            front = front->next;
            delete temp;
        }
    }

    bool isEmpty() {
        return count == 0;
    }

    // 入队：尾插
    void enQueue(int val) {
        Node* newNode = new Node(val);
        if (isEmpty()) {
            front = rear = newNode;
        } else {
            rear->next = newNode;
            rear = newNode;
        }
        count++;
    }

    // 出队：头删
    bool deQueue() {
        if (isEmpty()) {
            cout << "队列为空！" << endl;
            return false;
        }
        Node* temp = front;
        front = front->next;
        delete temp;
        count--;
        return true;
    }

    // 获取队头
    int getFront() {
        if (isEmpty()) return -1;
        return front->data;
    }

    int size() {
        return count;
    }

    void printQueue() {
        if (isEmpty()) {
            cout << "队列为空！" << endl;
            return;
        }
        Node* cur = front;
        cout << "链式队列：";
        while (cur != nullptr) {
            cout << cur->data << " ";
            cur = cur->next;
        }
        cout << endl;
    }
};

// 测试链式队列
int main() {
    LinkQueue q;

    q.enQueue(1);
    q.enQueue(2);
    q.enQueue(3);
    q.printQueue(); // 1 2 3

    q.deQueue();
    q.printQueue(); // 2 3

    cout << "队头：" << q.getFront() << endl; // 2
    return 0;
}
```

---

# 三、实际开发：直接用 C++ STL 队列
工作中不用手写，直接用标准库 `queue`，简单高效。

```cpp
#include <iostream>
#include <queue> // STL队列头文件
using namespace std;

int main() {
    queue<int> q;

    // 入队
    q.push(10);
    q.push(20);
    q.push(30);

    // 队头、队尾
    cout << "队头：" << q.front() << endl; // 10
    cout << "队尾：" << q.back() << endl;  // 30

    // 出队
    q.pop();
    cout << "出队后队头：" << q.front() << endl; // 20

    // 大小、判空
    cout << "大小：" << q.size() << endl; // 2
    cout << "是否为空：" << q.empty() << endl; // 0

    return 0;
}
```

---

# 四、notice
1. **循环队列**
   - 队空：`front == rear`
   - 队满：`(rear+1) % maxSize == front`
   - 必须空一个位置，否则无法区分空/满

2. **链式队列**
   - 入队：尾指针后移
   - 出队：头指针后移
   - 无容量上限

3. **STL queue 常用函数**
   - `push()` 入队
   - `pop()` 出队
   - `front()` 取队头
   - `empty()` 判空
   - `size()` 大小
