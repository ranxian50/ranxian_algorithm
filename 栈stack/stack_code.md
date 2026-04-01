下面给你一份**完整、可直接运行、注释详细**的 C++ 栈实现，包含：
数组实现栈 + 链式栈 + 测试代码，适合作业、复习、面试。

---

# 一、顺序栈（数组实现）Stack_array.h / 直接写在 main 里
```cpp
#include <iostream>
using namespace std;

// 顺序栈（数组实现）
template <class T>  // 模板，支持 int、double、char 等
class SeqStack {
private:
    T* data;        // 数组指针
    int top;        // 栈顶下标
    int maxSize;    // 栈最大容量

public:
    // 构造函数
    SeqStack(int size = 100) {
        maxSize = size;
        data = new T[maxSize];
        top = -1;   // 空栈
    }

    // 析构函数
    ~SeqStack() {
        delete[] data;
    }

    // 判断栈空
    bool isEmpty() const {
        return top == -1;
    }

    // 判断栈满
    bool isFull() const {
        return top == maxSize - 1;
    }

    // 入栈 push
    bool push(const T& x) {
        if (isFull()) {
            cout << "栈满，无法入栈！" << endl;
            return false;
        }
        data[++top] = x;
        return true;
    }

    // 出栈 pop
    bool pop(T& x) {
        if (isEmpty()) {
            cout << "栈空，无法出栈！" << endl;
            return false;
        }
        x = data[top--];
        return true;
    }

    // 取栈顶元素（不删除）
    bool getTop(T& x) const {
        if (isEmpty()) {
            cout << "栈空！" << endl;
            return false;
        }
        x = data[top];
        return true;
    }

    // 输出栈（从栈底到栈顶）
    void printStack() const {
        if (isEmpty()) {
            cout << "栈空" << endl;
            return;
        }
        cout << "栈内容（底→顶）：";
        for (int i = 0; i <= top; ++i) {
            cout << data[i] << " ";
        }
        cout << endl;
    }
};
```

## 测试 main
```cpp
int main() {
    SeqStack<int> st(10);

    st.push(1);
    st.push(2);
    st.push(3);
    st.printStack();

    int x;
    st.getTop(x);
    cout << "栈顶：" << x << endl;

    st.pop(x);
    cout << "出栈：" << x << endl;
    st.printStack();

    return 0;
}
```

---

# 二、链式栈（单链表实现）LinkStack.h
```cpp
#include <iostream>
using namespace std;

// 链栈节点
template <class T>
struct Node {
    T data;
    Node<T>* next;
};

// 链式栈
template <class T>
class LinkStack {
private:
    Node<T>* top;   // 栈顶指针

public:
    // 构造
    LinkStack() {
        top = nullptr;
    }

    // 析构
    ~LinkStack() {
        Node<T>* p;
        while (top != nullptr) {
            p = top;
            top = top->next;
            delete p;
        }
    }

    bool isEmpty() const {
        return top == nullptr;
    }

    // 入栈
    bool push(const T& x) {
        Node<T>* s = new Node<T>;
        s->data = x;
        s->next = top;   // 新节点指向原栈顶
        top = s;         // 栈顶上移
        return true;
    }

    // 出栈
    bool pop(T& x) {
        if (isEmpty()) {
            cout << "栈空！" << endl;
            return false;
        }
        Node<T>* p = top;
        x = p->data;
        top = top->next;
        delete p;
        return true;
    }

    // 取栈顶
    bool getTop(T& x) const {
        if (isEmpty()) {
            cout << "栈空！" << endl;
            return false;
        }
        x = top->data;
        return true;
    }

    // 输出
    void printStack() const {
        if (isEmpty()) {
            cout << "栈空" << endl;
            return;
        }
        Node<T>* p = top;
        cout << "栈内容（顶→底）：";
        while (p != nullptr) {
            cout << p->data << " ";
            p = p->next;
        }
        cout << endl;
    }
};
```

## 链式栈测试 main
```cpp
int main() {
    LinkStack<int> st;

    st.push(10);
    st.push(20);
    st.push(30);
    st.printStack();

    int x;
    st.pop(x);
    cout << "出栈：" << x << endl;
    st.printStack();

    return 0;
}
```

---

# 三、C++ STL 栈（实际开发用这个）
```cpp
#include <iostream>
#include <stack>   // STL栈
using namespace std;

int main() {
    stack<int> st;

    st.push(1);
    st.push(2);
    st.push(3);

    cout << "栈顶：" << st.top() << endl;

    st.pop();
    cout << "出栈后栈顶：" << st.top() << endl;

    cout << "是否为空：" << boolalpha << st.empty() << endl;
    cout << "元素个数：" << st.size() << endl;

    return 0;
}
```

---

# 四、核心操作总结（面试必背）
- `push(x)`：入栈 O(1)
- `pop(x)`：出栈 O(1)
- `getTop(x)`：取栈顶 O(1)
- `isEmpty()`：判空 O(1)
- 顺序栈：大小固定，访问快
- 链式栈：大小不限，略多内存

---

如果你需要，我可以再给你：
1. **栈实现括号匹配**（LeetCode20）完整 C++ 代码
2. **两个栈实现队列**
3. **最小栈（O(1)取最小值）**

你想要哪个？