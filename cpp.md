## IO
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

