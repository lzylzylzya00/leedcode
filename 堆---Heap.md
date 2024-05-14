堆---Heap

```c++

#pragma once
#include<stdio.h>
#include<assert.h>
#include<stdlib.h>
#include<string.h>

typedef int HPDataType;

typedef struct Heap
{
    HPDataType* a;  //a是指针
    int size;
    int capacity;
}Heap;


//堆的初始化
void HeapInit(Heap *hp){
    assert(hp);
    hp->a = NULL;
    hp->size = 0;
    hp->capacity = 0;
}

//堆的构建
void HeapCreate(Heap* hp,HPDataType* a,int n){
    assert(hp);
    assert(a);
    //开辟空间
    hp->a = (HPDataType*)malloc(sizeof(HPDataType) * n);
	if (hp->a == NULL)
	{
		perror("malloc fail");
		exit(-1);
	}
	hp->size = n;
	hp->capacity = n;
	memcpy(hp->a, a,sizeof(HPDataType) * n);
    //i = (n-1-1)/2 ;---父节点
    for (int i = (n-1-1)/2; i >= 0; i--)
    {
        //a 数组  n数组的个数  i 父节点
        AdjustDown(a,n,i);  //向下调整（时间复杂度O（n））
    }
    


}

//堆的摧毁
void HeapDestory(Heap* hp){
    assert(hp);
    free(hp->a);

    hp->a = NULL;

    hp->size = hp->capacity = 0;
}

//堆的插入  
//堆是一个完全二叉树，在插入元素时是在堆的末尾插入的，但是为了把一个元素插入后，使这个堆还是一个堆，我们需要对堆中的数据尽心再次调整
void HeapPush(Heap *hp,HPDataType x){
    assert(hp);
    if (hp->size == hp->capacity)  //堆的容量不够  扩容
    {
        int newcapacity = hp->capacity == 0 ? 4 : hp->capacity*2; 
        HPDataType* tem = (HPDataType*)realloc(hp->a,sizeof(HPDataType)*newcapacity);//开辟空间
        if (tem == NULL)  //异常处理
        {
            perror("malloc fail");
            exit(-1);
        }

        hp->a = tem;
        hp->capacity = newcapacity;
        
    }
    hp->a[hp->size] = x; //向最尾端插入
    hp->size++;//size 加一
    AdjustUp(hp->a,hp->size - 1); //调整    大堆 还是 小堆  看AdjustUp的实现
    
}

//堆的删除
void HeapPop(Heap* hp){
    assert(hp);
	assert(hp->size>0);
    Swap(&hp->a[0],&hp->a[hp->size-1]);
    --hp->size;
    AdjustDown(hp->a,hp->size,0);//小堆
}

//取堆顶数据
HPDataType HeapTop(Heap* hp);

//堆的数据个数
int HeapSize(Heap* hp);

//堆的数据判空
int HeapEmpty(Heap* hp);

//打印堆
void HeapPrint(Heap* hp){
    assert(hp);
    for (size_t i = 0; i < hp->size; i++)
    {
        printf("%d",hp->a[i]);
    }
    printf("\n");
    
}

//堆交换
void Swap(HPDataType* p1,HPDataType* p2){
    HPDataType temp = *p1;
	*p1 = *p2;
	*p2 = temp;
}


//向上调整（前面的是堆）
void AdjustUp(HPDataType* a,int child){
    int parent = (child -1)/2; //父节点
    while (child >0)
    {
        if (a[child] > a[parent]) //父节点大于子节点 那么交换  建立大堆
        {
            Swap(&a[child],&a[parent]);  //交换
            child = parent;    //
            parent = (parent -1)/2;
        }else{
            break;
        }
        
    }

} 

    //向下调整  n数组的个数  下标 n-1
    void AdjustDown(HPDataType* a,int n,int parent){
        int child = parent*2 + 1;
        while (child < n)
        {   
            if (child + 1 < n && a[child + 1] < a[child]) //选中一个最小的孩子
            {
                child++;
            }

            //建小堆
            if (a[child] < a[parent])
            {
                Swap(&a[child],&a[parent]);
                parent = child;
                child = parent*2 +1;
            }else{
                break;
            }
            
        }
        
    }
//堆排序
//1.将该数组建成一个大堆

//2.第一个数和最后一个数交换，然后把交换的那个较大的数不看做堆里面

//3.前n-1和数进行向下调整算法，选出大的数放到根节点，再跟倒数第二个交换......   
    void HeapSort(HPDataType* a,int n){
        int i = 0;
        //n 数组的个数  i 父节点
        for ( i = (n - 1 -1)/2; i >= 0; i--)
        {
            AdjustDown(a,n,i);
        }

        int end = n - 1;
        while (end > 0)
        {
            Swap(&a[0],&a[end]);
            AdjustUp(a,end);
            --end;
        }
        
        
    }
    

```

