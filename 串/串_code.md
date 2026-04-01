下面给你一份**完整、可直接编译运行、结构清晰**的 C++ 实现串（字符串），包含：
- 串的结构体定义
- 基本操作（赋值、求长、比较、连接、子串、查找、输出）
- **BF 暴力匹配**
- **KMP 算法 + next 数组**
- 主函数测试

全部用标准 C++ 写，不依赖复杂库，适合**数据结构作业/考试**。

---

# 串（String）C++ 完整实现

## 1. 串结构体定义（顺序串）
```cpp
#include <iostream>
#include <cstring>
using namespace std;

#define MAXSIZE 100

// 定长顺序存储表示
typedef struct {
    char ch[MAXSIZE];
    int length;
} SString;
```

---

## 2. 基本操作实现

### （1）赋值（把字符串常量赋给串）
```cpp
void StrAssign(SString &T, const char *chars) {
    int len = strlen(chars);
    if (len > MAXSIZE - 1) {
        cout << "串太长，溢出" << endl;
        return;
    }
    T.length = len;
    for (int i = 0; i < len; i++) {
        T.ch[i] = chars[i];
    }
}
```

### （2）求串长
```cpp
int StrLength(const SString &T) {
    return T.length;
}
```

### （3）输出串
```cpp
void StrPrint(const SString &T) {
    for (int i = 0; i < T.length; i++) {
        cout << T.ch[i];
    }
    cout << endl;
}
```

### （4）串比较
相等返回 0，S > T 返回正数，否则负数
```cpp
int StrCompare(const SString &S, const SString &T) {
    int minLen = min(S.length, T.length);
    for (int i = 0; i < minLen; i++) {
        if (S.ch[i] != T.ch[i]) {
            return S.ch[i] - T.ch[i];
        }
    }
    return S.length - T.length;
}
```

### （5）串连接
```cpp
bool Concat(SString &T, const SString &S1, const SString &S2) {
    if (S1.length + S2.length > MAXSIZE - 1) {
        cout << "连接后长度溢出" << endl;
        return false;
    }
    T.length = S1.length + S2.length;
    for (int i = 0; i < S1.length; i++) {
        T.ch[i] = S1.ch[i];
    }
    for (int i = 0; i < S2.length; i++) {
        T.ch[S1.length + i] = S2.ch[i];
    }
    return true;
}
```

### （6）求子串
pos 从 0 开始
```cpp
bool SubString(SString &Sub, const SString &S, int pos, int len) {
    if (pos < 0 || pos >= S.length || len < 0 || pos + len > S.length) {
        cout << "子串位置或长度不合法" << endl;
        return false;
    }
    Sub.length = len;
    for (int i = 0; i < len; i++) {
        Sub.ch[i] = S.ch[pos + i];
    }
    return true;
}
```

---

## 3. 模式匹配算法

### （1）BF 算法（暴力匹配）
返回第一次匹配位置，失败返回 -1
```cpp
int Index_BF(const SString &S, const SString &T) {
    int n = S.length;
    int m = T.length;
    int i = 0, j = 0;

    while (i < n && j < m) {
        if (S.ch[i] == T.ch[j]) {
            i++;
            j++;
        } else {
            i = i - j + 1;
            j = 0;
        }
    }
    if (j == m) {
        return i - j;
    }
    return -1;
}
```

### （2）KMP：求 next 数组
```cpp
void GetNext(const SString &T, int next[]) {
    int j = 0, k = -1;
    next[0] = -1;

    while (j < T.length - 1) {
        if (k == -1 || T.ch[j] == T.ch[k]) {
            j++;
            k++;
            next[j] = k;
        } else {
            k = next[k];
        }
    }
}
```

### （3）KMP 匹配
```cpp
int Index_KMP(const SString &S, const SString &T) {
    int n = S.length;
    int m = T.length;
    int next[MAXSIZE];
    GetNext(T, next);

    int i = 0, j = 0;
    while (i < n && j < m) {
        if (j == -1 || S.ch[i] == T.ch[j]) {
            i++;
            j++;
        } else {
            j = next[j];
        }
    }
    if (j == m) {
        return i - j;
    }
    return -1;
}
```

---

## 4. 主函数测试
```cpp
int main() {
    SString S, T, Sub, Cat;

    // 赋值
    StrAssign(S, "abcabcabcd");
    StrAssign(T, "abcd");

    cout << "主串 S: "; StrPrint(S);
    cout << "模式串 T: "; StrPrint(T);

    // BF 匹配
    int posBF = Index_BF(S, T);
    cout << "BF 匹配位置: " << posBF << endl;

    // KMP 匹配
    int posKMP = Index_KMP(S, T);
    cout << "KMP 匹配位置: " << posKMP << endl;

    // 求子串
    SubString(Sub, S, 3, 4);
    cout << "S 从 3 开始长度 4 的子串: "; StrPrint(Sub);

    // 串连接
    Concat(Cat, S, T);
    cout << "S+T: "; StrPrint(Cat);

    return 0;
}
```

---

## 运行结果示例
```
主串 S: abcabcabcd
模式串 T: abcd
BF 匹配位置: 6
KMP 匹配位置: 6
S 从 3 开始长度 4 的子串: abca
S+T: abcabcabcdabcd
```

---

如果你需要，我可以再给你：
- **next 数组手工计算步骤**
- **KMP 完整图解**
- **带注释的极简版（考试背诵用）**

你想要哪个？