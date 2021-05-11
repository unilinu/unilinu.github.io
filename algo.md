## 排序

### quicksort

- [partition](https://www.cnblogs.com/TenosDoIt/p/3665038.html)

  - 第一种

    ```cpp
    int mypartition(vector<int>&arr, int low, int high)
    {
        int pivot = arr[low];//选第一个元素作为枢纽元
        while(low < high)
        {
            while(low < high && arr[high] >= pivot)high--;
            arr[low] = arr[high];//从后面开始找到第一个小于pivot的元素，放到low位置
            while(low < high && arr[low] <= pivot)low++;
            arr[high] = arr[low];//从前面开始找到第一个大于pivot的元素，放到high位置
        }
        arr[low] = pivot;//最后枢纽元放到low的位置
        return low;
    }
    ```

  - 第二种

    ```cpp
    int mypartition(vector<int>&arr, int low, int high)
    {
        int pivot = arr[high];//选最后一个元素作为枢纽元
        int location = low-1;//location指向比pivot小的元素段的尾部
        for(int i = low; i < high; i++)//比枢纽元小的元素依次放在前半部分
           if(arr[i] < pivot)
               swap(arr[i], arr[++location]);
        swap(arr[high], arr[location+1]);
        return location+1;
    } 
    
    
    ```

  - STL

    ```cpp
    // https://en.cppreference.com/w/cpp/algorithm/partition
    template<class ForwardIt, class UnaryPredicate>
    ForwardIt partition(ForwardIt first, ForwardIt last, UnaryPredicate p)
    {
        first = std::find_if_not(first, last, p);
        if (first == last) return first;
     
        for (ForwardIt i = std::next(first); i != last; ++i) {
            if (p(*i)) {
                std::iter_swap(i, first);
                ++first;
            }
        }
        return first;
    }
    // Example
    #include <algorithm>
    template <class ForwardIt>
     void quicksort(ForwardIt first, ForwardIt last)
     {
        if(first == last) return;
        auto pivot = *std::next(first, std::distance(first,last)/2);
        ForwardIt middle1 = std::partition(first, last, 
                             [pivot](const auto& em){ return em < pivot; });
        ForwardIt middle2 = std::partition(middle1, last, 
                             [pivot](const auto& em){ return !(pivot < em); });
        quicksort(first, middle1);
        quicksort(middle2, last);
     }
    auto it = std::partition(v.begin(), v.end(), [](int i){return i % 2 == 0;});
    ```

    

- [几种实现](https://segmentfault.com/a/1190000004410119)

  