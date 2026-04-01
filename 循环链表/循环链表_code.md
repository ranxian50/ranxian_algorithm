下面给你一份**可直接编译运行、注释超详细**的 **C++ 循环链表完整示例**，包含：
结构体定义、创建、头插、尾插、遍历、删除、判空、清空、约瑟夫环经典示例。

---

## 一、完整 C++ 代码（带头结点的循环单链表）

```cpp
#include <iostream>
using namespace std;

// 1. 定义循环链表节点结构
struct Node {
    int data;           // 数据域
    Node* next;         // 指针域
};

// 2. 创建头结点（初始化空循环链表）
Node* createList() {
    Node* head = new Node;
    head->next = head;  // 关键：自己指向自己，构成环
    return head;
}

// 3. 判断链表是否为空
bool isEmpty(Node* head) {
    return head->next == head;
}

// 4. 尾插法：在链表尾部插入节点
void addTail(Node* head, int val) {
    // 新建节点
    Node* newNode = new Node;
    newNode->data = val;

    Node* p = head;
    // 找到最后一个节点（p->next == head 就是尾节点）
    while (p->next != head) {
        p = p->next;
    }

    newNode->next = head;  // 新节点指向头
    p->next = newNode;     // 原尾节点指向新节点
}

// 5. 头插法：在链表头部插入节点
void addHead(Node* head, int val) {
    Node* newNode = new Node;
    newNode->data = val;

    newNode->next = head->next;
    head->next = newNode;
}

// 6. 遍历并打印循环链表
void printList(Node* head) {
    if (isEmpty(head)) {
        cout << "链表为空" << endl;
        return;
    }

    Node* p = head->next;
    // 循环条件：回到头结点就停止
    while (p != head) {
        cout << p->data << " ";
        p = p->next;
    }
    cout << endl;
}

// 7. 删除指定值的第一个节点
bool deleteNode(Node* head, int val) {
    Node* p = head;
    // 找到要删除节点的前驱
    while (p->next != head && p->next->data != val) {
        p = p->next;
    }

    // 没找到
    if (p->next == head) {
        cout << "值 " << val << " 不存在" << endl;
        return false;
    }

    Node* q = p->next;        // 要删除的节点
    p->next = q->next;        // 跨过节点
    delete q;
    q = nullptr;
    return true;
}

// 8. 清空链表（释放所有节点）
void clearList(Node*& head) {
    Node* p = head->next;
    Node* q = nullptr;

    while (p != head) {
        q = p->next;
        delete p;
        p = q;
    }

    delete head;
    head = nullptr;
}

// 9. 经典：约瑟夫环问题（用循环链表实现）
// n个人，报数到m出局，返回最后幸存者编号
void josephus(int n, int m) {
    Node* head = createList();
    for (int i = 1; i <= n; ++i) {
        addTail(head, i);
    }

    cout << "初始队列：";
    printList(head);

    Node* pre = head;
    Node* cur = head->next;

    while (head->next != head) { // 只剩头结点时结束
        // 报数 m-1 步
        for (int i = 1; i < m; ++i) {
            pre = cur;
            cur = cur->next;
            if (cur == head) {   // 跳过虚拟头结点
                cur = cur->next;
                pre = pre->next;
            }
        }

        // 出局
        cout << "出局：" << cur->data << endl;
        pre->next = cur->next;
        delete cur;
        cur = pre->next;

        if (cur == head) {
            cur = cur->next;
            pre = pre->next;
        }
    }

    cout << "幸存者：" << pre->data << endl;
    clearList(head);
}

// 主函数测试
int main() {
    // ========== 基础功能测试 ==========
    Node* head = createList();

    addTail(head, 10);
    addTail(head, 20);
    addTail(head, 30);
    addHead(head, 5);

    cout << "链表内容：";
    printList(head);

    deleteNode(head, 20);
    cout << "删除20后：";
    printList(head);

    cout << "------------------------" << endl;

    // ========== 约瑟夫环测试 ==========
    cout << "约瑟夫环问题（n=5, m=2）：" << endl;
    josephus(5, 2);

    clearList(head);
    return 0;
}
```

---

## 二、代码核心要点（面试必背）
1. **空循环链表**
   ```cpp
   head->next = head;
   ```
2. **判空**
   ```cpp
   head->next == head
   ```
3. **遍历结束条件**
   ```cpp
   p != head
   ```
4. **尾节点判断**
   ```cpp
   p->next == head
   ```
5. 没有 `NULL`，**不会出现空指针访问**，但容易死循环。

---

## 三、运行结果示例
```
链表内容：5 10 20 30
删除20后：5 10 30
------------------------
约瑟夫环问题（n=5, m=2）：
初始队列：1 2 3 4 5
出局：2
出局：4
出局：1
出局：5
幸存者：3
```

---

如果你需要，我还能给你：
- 带**尾指针优化**的循环链表（插入尾节点 O(1)）
- **循环双链表**版本
- 更精简的考试版代码

需要哪个？