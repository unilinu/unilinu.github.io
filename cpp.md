
## TODO

- [Static全局变量与普通全局变量 & Static关键字的不同含义 ](https://blog.csdn.net/weiyuefei/article/details/51563890)
- https://www.cnblogs.com/clover-toeic/p/3728026.html
- 
## C++ Thinking

[Bjarne Stroustrup's FAQ](https://www.stroustrup.com/bs_faq.html)

[Bjarne Stroustrup's FAQ（中文版）](https://www.stroustrup.com/bsfaqcn.html)

[Bjarne Stroustrup 的 C++ 风格与技术 FAQ（中文版）](https://www.stroustrup.com/bsfaq2cn.html)

[C++ Core Guidelines](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md)

## C++ Traps
### IO
- [read a string line](https://blog.csdn.net/lwgkzl/article/details/53232889)
	1. `getline(cin, string); // 注意cin>>不读取分隔符`
	1. `cin.getline(str*, len); // 读取分隔符\n`
	1. `cin.get(str*, len).get(); // 不读取分隔符\n`

### QUEUE

- [priority_queue](https://blog.csdn.net/weixin_36888577/article/details/79937886)

    1. **优先级队列，优先级越大越优先，越先出队**
    2. **也即是，队首优先级最高**
    3. **默认数值越大，优先级越大**
    4. **为了理解，可以认为，比较器第一个参数优先级低于第二个参数**

```cpp
// 最大顶堆
priority_queue <int,vector<int>,less<int> >q; // 默认类型
// 最小顶堆
priority_queue <int,vector<int>,greater<int> > q;

q.top() //访问队头元素
q.pop // 弹出队头元素
q.swap // 交换内容
```

### ALGO 
- sort()

```
class TestIndex{  
public:  
    int index;  
    TestIndex(){  
    }  
    TestIndex(int _index):index(_index){  
    }  
    bool operator()(const TestIndex* t1,const TestIndex* t2){  
        printf("Operator():%d,%d/n",t1->index,t2->index);  
        return t1->index < t2->index;  
    }  
    bool operator < (const TestIndex& ti) const {  
        printf("Operator<:%d/n",ti.index);  
        return index < ti.index;  
    }  
};  
bool compare_index(const TestIndex* t1,const TestIndex* t2){  
    printf("CompareIndex:%d,%d/n",t1->index,t2->index);  
    return t1->index < t2->index;  
}  
int main(int argc, char** argv) {  
    list<TestIndex*> tiList1;  
    list<TestIndex> tiList2;  
    vector<TestIndex*> tiVec1;  
    vector<TestIndex> tiVec2;  
    TestIndex* t1 = new TestIndex(2);  
    TestIndex* t2 = new TestIndex(1);  
    TestIndex* t3 = new TestIndex(3);  
    tiList1.push_back(t1);  
    tiList1.push_back(t2);  
    tiList1.push_back(t3);  
    tiList2.push_back(*t1);  
    tiList2.push_back(*t2);  
    tiList2.push_back(*t3);  
    tiVec1.push_back(t1);  
    tiVec1.push_back(t2);  
    tiVec1.push_back(t3);  
    tiVec2.push_back(*t1);  
    tiVec2.push_back(*t2);  
    tiVec2.push_back(*t3);  
    printf("tiList1.sort()/n");  
    tiList1.sort();//无法正确排序  
    printf("tiList2.sort()/n");  
    tiList2.sort();//用<比较  
    printf("tiList1.sort(TestIndex())/n");  
    tiList1.sort(TestIndex());//用()比较  
    printf("sort(tiVec1.begin(),tiVec1.end())/n");  
    sort(tiVec1.begin(),tiVec1.end());//无法正确排序  
    printf("sort(tiVec2.begin(),tiVec2.end())/n");  
    sort(tiVec2.begin(),tiVec2.end());//用<比较  
    printf("sort(tiVec1.begin(),tiVec1.end(),TestIndex())/n");  
    sort(tiVec1.begin(),tiVec1.end(),TestIndex());//用()比较  
    printf("sort(tiVec1.begin(),tiVec1.end(),compare_index)/n");  
    sort(tiVec1.begin(),tiVec1.end(),compare_index);//用compare_index比较  
    return 0;  
｝ 
```



1. list

   > You can't use `std::sort` to sort `std::list`, because `std::sort` requires iterators to be random access, and `std::list` iterators are only bidirectional.
   >
   > However, `std::list` has a member function `sort` that will sort it.

2. 

## C++ MUST KNOW

### 纯虚函数和抽象类

- 纯虚函数：没有函数体的虚函数, virtual void show() = 0;
- 抽象类：包含纯虚函数的类

1. 如果我们不在派生类中覆盖纯虚函数，那么派生类也会变成抽象类
2. 构造函数不能是虚函数，而析构函数可以是虚析构函数
3. 当基类指针指向派生类对象并删除对象时，我们可能希望调用适当的析构函数。 如果析构函数不是虚拟的，则只能调用基类析构函数。

> 关于C++为什么不支持虚拟构造函数，Bjarne很早以前就在C++
Style and Technique FAQ里面做过回答：A
virtual call is a mechanism to get work done given partial
information. In particular, "virtual" allows us to call a
function knowing only an interfaces and not the exact type of the
object. To create an object you need complete information. In
particular, you need to know the exact type of what you want to
create. Consequently, a "call to a constructor" cannot be
virtual.

> Bjarne建议的解决方案是factory pattern，也就是为每一个要构建的类型再创建一个对应的factory，把问题放到factory的make方法中去解决。这也是C++中的通用解决方案。

### CONST

- 常量，常量表达式`constexpr`

  const定义的变量只有类型为整数或枚举，且以常量表达式初始化时才能作为常量表达式。

- 常类型，`const`限定，只能读的变量，尤其是引用

  其他情况下它只是一个 **const 限定的变量**，不要与常量混淆。

### [头文件和源文件写什么](https://blog.csdn.net/lyanliu/article/details/2195632)

#### 头文件

写类的定义（包括类里面的成员和方法的声明）、函数原型、#define常数等，但一般来说不写出具体的实现。

在写头文件时需要注意，在开头和结尾处**必须**按照如下样式加上预编译语句：

```cpp
 #ifndef CLASS_NAME_H
 #define  CLASS_NAME_H

 // coding here

 #endif
```

#### 源文件

源文件主要写实现头文件中已经声明的那些函数的具体代码。

需要注意的是，开头必须#include一下实现的头文件，以及要用到的头文件。

那么当你需要用到自己写的头文件中的类时，只需要#include进来就行了。





### [STATIC](https://www.cnblogs.com/nzbbody/p/3413169.html)

1. 为什么设计static？考虑下面的需求：**在程序运行过程中**，在一个范围内，有一个对象大家共享，而且可以多次使用，状态能够保持，对象的生命周期一直持续到程序运行结束。

2. 静态对象要分配在全局数据区，程序运行期间，不能释放，一直到程序终止。

3. 静态对象的生命周期是程序的整个运行过程。但是可以限定静态对象的作用域，根据作用域的大小，可分为静态局部对象，静态全局对象。静态局部对象是指方法内的静态对象，静态全局对象是指编译单元里的静态对象。（注意：生命周期是时间概念，作用域是空间概念）

4. 静态对象只能初始化一次。**严格来讲，任何对象都只能初始化一次，而且是在定义的时候。**后面再想修改对象的值，只能通过赋值操作。什么叫初始化，什么叫赋值？初始化是指创建对象的时候（也就是定义的时候），给对象设置初始值。赋值是指对象已经存在值了，擦除当前值，使用新值代替。任何对象都只创建一次，也就是初始化一次。

5. 那为什么方法内的局部对象可以初始化多次呢？实际上，方法内的对象也只是初始化一次。第一次调用方法，初始化对象，方法退出，对象销毁。第二次调用方法，又初始化对象，但是，这时的对象不是第一次调用时的对象，而且没有任何关系。

6. C++可以声明多次，只能定义一次，但是有一些例外。static就是一例，static可以在多个编译单元里定义，这是因为static是内部链接。全局static的作用域是当前的编译单元。不同编译单元都有专属于自己的一份static对象，而且彼此没有关系，不会保持相同的值。

7. 对于类，头文件是对类的定义，但是是对类的成员的声明。类中static成员，在类定义中声明，在类定义外，需要定义，也就是初始化。类中static成员的作用域是当前类，所有类对象共享。

8. 声明：是说我有这个东西。定义：创建这个东西，并初始化，说明这个东西就在这里。
9. 基本类型对象：**static对象，定义的时候，没有显示初始化，会被隐式初始化为0或者null; **动态对象（方法内的对象，在栈上分配）定义的时候，没有显示初始化，可以认为隐式初始化为一个随机值。

### [声明VS定义](https://blog.csdn.net/cloud323/article/details/75646379)

#### 声明与定义的区别

声明是将一个名称引入程序。

定义提供了一个实体在程序中的唯一描述，涉及到内存空间的分配以及初始值的设定。

声明和定义有时是同时存在的。

- 定义也是声明

    ```cpp
    int a = 10;    //定义就是声明
    // extern声明不是定义，即不分配存储空间。
    extern int b;  //声明，不是定义
    //注意：如果使用extern关键字时，对变量进行了初始化，那就是定义。
    extern int b = 20;  //是定义
    ```

- 声明仅仅是声明

    ```cpp
    1. 仅仅提供函数原型：void display();
    2. extern int a;
    3. class A;
    4. typedef 声明;
    5. 在类中定义的静态数据成员的声明

    例如：
    class A{
    public:
        static int a;  //声明
    };
    ```

- 定义仅仅是定义

  ```
  1:  在类定义之外，定义并初始化一个静态数据成员。如 int A::a = 0;
  2:  在类外定义非内联成员函数。
  ```

  

#### 内部链接与外部链接

在编译时，编译器只检测程序语法和函数、变量是否被声明。如果函数未被声明，编译器会给出一个警告，但可以生成目标文件。而在链接程序时，链接器会在所有的目标文件中找寻函数的实现。如果找不到，那到就会报链接错误码。链接把不同编译单元产生的符号联系起来。有两种链接方式：内部链接和外部链接。

- 内部链接：

  如果一个符号名对于它的编译单元来说是局部的，并且在链接时不可能与其他编译单元中的同样的名称相冲突，那个这个符号就是内部链接。内部链接意味着对此符号的访问仅限于当前的编译单元中，对其他编译单元都是不可见的。

  

- 外部链接：

   在一个多文件的程序中，如果一个符号在链接时可以和其他编译单元交互，那么这个名称就有外部链接。外部链接意味着该定义不仅仅局限在单个编译单元中。

  

- 函数与变量具有的连接性

    全局变量、非内联成员函数、非内联函数、非静态自由函数都具有外部链接。

    使用const、static关键字声明的函数或变量具有内部链接。

  

- 在头文件中可以包含的内容

  声明仅仅是将一个符号引入到一个作用域。而定义提供了一个实体在程序中的唯一描述。

  1. 在一个给定的作用域中重复声明一个符号是可以的，但是却不能重复定义，否则将会引起编译错误。

  2. 将具有外部链接的定义放在头文件中几乎都是编程错误。因为如果该头文件中被多个源文件包含，那么就会存在多个定义，链接时就会出错。

  3. 在头文件中放置内部链接的定义却是合法的，但不推荐使用的。因为头文件被包含到多个源文件中时，在每个编译单元中有自己的实体存在。大量消耗内存空间，还会影响机器性能。

## Procedural Programming

- local static object

- inline function

- template function

  ```
  template <typename T>
  void func(T t){
  	// something
  }
  ```

## Generic Programming

- Container

  ```cpp
  // common operations
  empty(); size(); clear();
  == != =
  
  ```

  - Sequential Container

    - vector

      ```cpp
      #include <vector>
      
      void push_back(); void pop_back();
      // push_front(); pop_front();
      type front(); type back();
      
      iterator insert(iterator position, type value); // 4 forms
      iterator erase(iterator position); // 2 forms
      
      ```
    - list
  
      ```cpp
      #include <list>
      
      push_back(); pop_back();
      push_front(); pop_front();
      type front(); type back();
      
      iterator insert(iterator position, type value); // 4 forms
      iterator erase(iterator position); // 2 forms
      ```
      
    - deque
    
      ```cpp
      #include <deque>
      
      push_back(); pop_back();
      push_front(); pop_front();
      type front(); type back();
      
      iterator insert(iterator position, type value); // 4 forms
      iterator erase(iterator position); // 2 forms
      ```
- Iterator (generic pointer)

  ```cpp
  begin(); end(); 
  * -> ++ == !=
      
  while(first!=last){
      cout<<*first<<endl;
      ++first;
  }
  for(vector<type>::const_iterator iter = vec.begin(); 
      iter != vec.end(); ++iter){
      
  }
  ```


- Generic Algorithms

  ```cpp
  #include <algorithm>
  
  // search 
  find(); 
  count(); 
  find_if(); 
  binary_search(); 
  find_first_of(); adjacent_find();
  // sorting and ordering
  sort(); 
  partial_sort(); 
  merge(); 
  partition(); 
  reverse(); 
  rotate(); random_shuffle();
  // copy and deletion and substitution
  copy(); remove(); remove_if(); replace(); replace_if(); swap(); unique();
  // relations
  equal(); includes(); mismatch();
  // generation and mutation
  fill(); for_each(); generate(); transform();
  // numeric 
  partial_sum(); adjacent_difference(); accmulate(); inner_product();
  // set 
  set_union(); set_difference();
  
  ```


- Functor

  ```
  // arithmetic
  plus<type>; minus<type>; negate<type>; multiplies<type>; divides<type>; modules<type>; 
  // relational
  less<type>; less_equal<type>; greater<type>; greater_equal<type>; equal_to<type>; not_equal_to<type>; 
  // logical
  logical_and<type>; logical_or<type>; logical_not<type>; 
  
  ```

  
