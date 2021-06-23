## SKILLS 

### 面试

- https://blog.csdn.net/high2011/article/details/59481137
- 简历时间连贯协调一致（自行理解）
- 

### 智力题

- https://blog.csdn.net/qq_43518645/article/details/104118528

- https://kknews.cc/news/remy8qn.html

  

## OS

### 如何应对服务端高并发？

高并发问题的本质就是：资源的有限性

基本原则：分而治之，并提高单个请求的处理速度

1. 从客户端看
   - 尽量减少请求数量，比如：依靠客户端自身的缓存或处理能力
   - 尽量减少对服务端资源的不必要耗费，比如：重复使用某些资源，如连接池客户端处理的基本原则就是：能不访问服务端就不要访问

2. 从服务端看

    - 增加资源供给，比如：更大的网络带宽，使用更高配置的服务器，使用高性能的Web服务器，使用高性能的数据库
    - 请求分流，比如：使用集群,分布式的系统架构
    - 应用优化，比如：使用更高效的编程语言,优化处理业务逻辑的算法,优化访问数据库的SQL

## CPP

### 1. 实现String类的默认构造函数、默认拷贝构造函数、默认赋值函数

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <cassert>

using namespace std;

class String {
public:
    int len;
    char *buf;

    String() : len(0), buf(nullptr) { cout << "String()" << endl; }

    explicit String(int length) : len(length), buf(nullptr) {
        cout << "String(int)" << endl;
        buf = new char[length];
    }

    String(const String &other) : len(other.len), buf(nullptr) {
        cout << "String(String &)" << endl;
        if (other.buf == nullptr) return;
        buf = new char[other.len];
        for (int i = 0; i < other.len; ++i)
            buf[i] = other.buf[i];
    }

    explicit String(char *arr) : len(0), buf(nullptr) {
        cout << "String(char *)" << endl;
        for (int i = 0; arr[i] != '\0'; ++i)
            ++len;
        if (len == 0) return;
        buf = new char[len];
        for (int i = 0; i < len; ++i) {
            buf[i] = arr[i];
        }
    }

    String &operator=(const String &other) {
        cout << "=" << endl;
        if (this == &other) return *this;
        delete[] buf;

        if (other.buf == nullptr) {
            len = 0;
            buf = nullptr;
            return *this;
        }


        len = other.len;
        buf = new char[other.len];
        for (int i = 0; i < len; ++i) {
            buf[i] = other.buf[i];
        }
        return *this;
    }

    ~String() {
        cout << "~string" << endl;
        delete[] buf;
    }
};

int main() {
    // some initials
    int len = 10;
    char arr[] = "abc";
    char *p = new char[len]{'a', 'b', 'c'};

    // initialize Strings
    String str1dft;
    String str2len(len);
    String str3arr(arr);
    String str4pit(p);
    String str5str(str3arr);

    assert((str1dft.len == 0) && (str1dft.buf == nullptr));
    assert((str2len.len == len) && (str2len.buf != nullptr));
    assert((str3arr.len == 3) && (str3arr.buf != nullptr));
    assert((str4pit.len == 3) && (str4pit.buf != nullptr));
    assert((str5str.len == 3) && (str5str.buf != nullptr));
    str1dft = str1dft;
    assert((str1dft.len == 0) && (str1dft.buf == nullptr));
    str3arr = str1dft;
    assert((str3arr.len == 0) && (str3arr.buf == nullptr));
    str1dft = str3arr = str4pit;
    assert((str1dft.len == 3) && (str1dft.buf != str3arr.buf));

    delete[] p; // new with delete

    cout<<"start String []"<<endl;
    auto *sp = new String[3];
    assert(sp->len == 0 && sp->buf == nullptr);
    delete[] sp; // will destroy heap data
    cout<<"end String []"<<endl;
    return 0;
}
```



