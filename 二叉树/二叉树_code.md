# C++ 二叉树完整实现（零基础可直接运行）
我给你写**可直接编译运行**的 C++ 完整代码，包含：
节点定义、创建二叉树、**4 种遍历（递归+非递归）**、求高度、求节点数、层序遍历、销毁树。

代码全部带注释，复制就能用！

---

## 完整 C++ 代码
```cpp
#include <iostream>
#include <queue>    // 层序遍历用
#include <stack>    // 非递归遍历用
#include <algorithm> // max函数
using namespace std;

// 1. 二叉树节点结构定义
struct TreeNode {
    int val;         // 节点值
    TreeNode* left;  // 左孩子
    TreeNode* right; // 右孩子

    // 构造函数
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

// 2. 创建一棵简单的二叉树（手动创建）
TreeNode* createTree() {
    // 构造如下树：
    //        1
    //       / \
    //      2   3
    //     / \
    //    4   5
    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(3);
    root->left->left = new TreeNode(4);
    root->left->right = new TreeNode(5);
    return root;
}

// ------------------- 递归遍历（最简单）-------------------
// 前序遍历：根 → 左 → 右
void preOrder(TreeNode* root) {
    if (!root) return;
    cout << root->val << " ";   // 根
    preOrder(root->left);       // 左
    preOrder(root->right);      // 右
}

// 中序遍历：左 → 根 → 右
void inOrder(TreeNode* root) {
    if (!root) return;
    inOrder(root->left);
    cout << root->val << " ";
    inOrder(root->right);
}

// 后序遍历：左 → 右 → 根
void postOrder(TreeNode* root) {
    if (!root) return;
    postOrder(root->left);
    postOrder(root->right);
    cout << root->val << " ";
}

// ------------------- 非递归遍历（面试必考）-------------------
// 非递归前序
void preOrderNonRec(TreeNode* root) {
    if (!root) return;
    stack<TreeNode*> st;
    st.push(root);
    while (!st.empty()) {
        TreeNode* node = st.top();
        st.pop();
        cout << node->val << " ";
        if (node->right) st.push(node->right); // 右先入栈
        if (node->left) st.push(node->left);   // 左后入栈
    }
}

// 非递归中序
void inOrderNonRec(TreeNode* root) {
    if (!root) return;
    stack<TreeNode*> st;
    TreeNode* cur = root;
    while (cur || !st.empty()) {
        while (cur) {   // 一直往左走
            st.push(cur);
            cur = cur->left;
        }
        cur = st.top();
        st.pop();
        cout << cur->val << " ";
        cur = cur->right;
    }
}

// 非递归后序
void postOrderNonRec(TreeNode* root) {
    if (!root) return;
    stack<TreeNode*> st1, st2;
    st1.push(root);
    while (!st1.empty()) {
        TreeNode* node = st1.top();
        st1.pop();
        st2.push(node);
        if (node->left) st1.push(node->left);
        if (node->right) st1.push(node->right);
    }
    while (!st2.empty()) {
        cout << st2.top()->val << " ";
        st2.pop();
    }
}

// ------------------- 层序遍历（BFS）-------------------
void levelOrder(TreeNode* root) {
    if (!root) return;
    queue<TreeNode*> q;
    q.push(root);
    while (!q.empty()) {
        TreeNode* node = q.front();
        q.pop();
        cout << node->val << " ";
        if (node->left) q.push(node->left);
        if (node->right) q.push(node->right);
    }
}

// ------------------- 常用操作 -------------------
// 求树的高度
int getHeight(TreeNode* root) {
    if (!root) return 0;
    return 1 + max(getHeight(root->left), getHeight(root->right));
}

// 求节点总数
int countNodes(TreeNode* root) {
    if (!root) return 0;
    return 1 + countNodes(root->left) + countNodes(root->right);
}

// 销毁二叉树（释放内存）
void destroyTree(TreeNode* root) {
    if (!root) return;
    destroyTree(root->left);
    destroyTree(root->right);
    delete root;
}

// ------------------- 主函数测试 -------------------
int main() {
    TreeNode* root = createTree();

    cout << "前序遍历（递归）："; preOrder(root); cout << endl;
    cout << "中序遍历（递归）："; inOrder(root); cout << endl;
    cout << "后序遍历（递归）："; postOrder(root); cout << endl;

    cout << "前序遍历（非递归）："; preOrderNonRec(root); cout << endl;
    cout << "中序遍历（非递归）："; inOrderNonRec(root); cout << endl;
    cout << "后序遍历（非递归）："; postOrderNonRec(root); cout << endl;

    cout << "层序遍历："; levelOrder(root); cout << endl;
    cout << "树的高度：" << getHeight(root) << endl;
    cout << "节点总数：" << countNodes(root) << endl;

    destroyTree(root);
    return 0;
}
```

---

## 运行结果
```
前序遍历（递归）：1 2 4 5 3
中序遍历（递归）：4 2 5 1 3
后序遍历（递归）：4 5 2 3 1
前序遍历（非递归）：1 2 4 5 3
中序遍历（非递归）：4 2 5 1 3
后序遍历（非递归）：4 5 2 3 1
层序遍历：1 2 3 4 5
树的高度：3
节点总数：5
```

---

## 代码核心说明
### 1. 节点结构
- `val`：存数据
- `left`、`right`：指向左右孩子
- 构造函数方便创建节点

### 2. 遍历口诀
- **前序**：根左右
- **中序**：左根右
- **后序**：左右根
- **层序**：从上到下一层一层（队列实现）

### 3. 非递归遍历
- 用**栈**模拟递归
- 面试高频考点，必须会写

### 4. 必备操作
- 求高度：递归找左右子树最大高度
- 求节点数：递归累加
- 销毁树：后序方式释放内存（避免野指针）

---

## 你可以接着让我做：
1. 二叉搜索树（BST）增删改查 C++ 实现
2. 平衡二叉树（AVL）C++ 实现
3. 堆（完全二叉树）C++ 实现
4. LeetCode 二叉树高频题解

需要我继续写哪一部分？