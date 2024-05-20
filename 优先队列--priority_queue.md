优先队列--priority_queue

## demo

```c++

#include<queue>
int my_array[10] = {5,3,8,9,10,45,62,5,8,56};

int main()
{   

    priority_queue<int,vector<int>,less<int>>  greaterHeap;  //大堆a

    priority_queue<int,vector<int>,greater<int>>  smallHeap;  //小堆

    for (int i = 0; i < 10; i++)
    {
        smallHeap.push(my_array[i]); //放入队列
    }

    for (int i = 0; i < 10; i++)
    {
        cout << "order =" << smallHeap.top() << endl;
        smallHeap.pop(); //删除堆顶
    }
    
    

    return 0;
}
```

## 概念

什么是priority_queue

优先队列是一种容器适配器，采用了堆这样的数据结构，保证了第一个元素总是整个优先队列中***\*最大的\****(或***\*最小的\****)元素

优先队列默认使用***\*vector\****作为底层存储数据的容器，在**vector**上使用了堆算法将**vector**中的元素构造成堆的结构，所以其实我们就可以把它当作堆，凡是需要用堆的位置，都可以考虑优先队列



## 声明参数

priority_queue的参数模板

```c++
template <class T, class Container = vector<T>,

                              class Compare = less<typename Container::value_type> >

class priority_queue;

```

 ***\*class T：T是优先队列中存储的元素的类型。\****



 ***\*class Container = vector<T>：Container是优先队列底层使用的存储结构，可以看出来，默认采用vector\****





***\*class Compare = less<typename Container::value_type>\**** ：***\*Compar是定义优先队列中元素的比较方式的类\****

**---- 默认就是  less  大堆   如果需要创建小堆，就需要将less改为greater。**



接下来看一下less类和greater类的函数参数

```c++
template <class T> 
        struct less : binary_function <T,T,bool> {
          bool operator() (const T& x, const T& y) const {return x<y;}
};

```





```c++
template <class T> 
        struct greater : binary_function <T,T,bool> {
          bool operator() (const T& x, const T& y) const {return x>y;}
}; 

```



## 内置方法

void pop()   --- 删除堆顶元素

const value_type& top()      ---  返回堆顶元素

push(const value_type& val)  --  增加元素

bool empty() const   ---  判空

size()   ---  大小