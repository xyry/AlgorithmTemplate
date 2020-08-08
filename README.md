[TOC]



# AlgorithmTemplate

记录做题用的模板,免得每次都现写

## 快速排序
日期:2020年08月07日10:36:47

不稳定排序

适用题目

1. acwing 785
2. acwing 786

```c++
//复杂度O(nlogn)
void quick_sort(int q[],int l,int r){
    if(l>=r) return;
    //i设置为l-1左端点左边 j设置为r+1右端点右边
    //所以后面处理的时候要先对i,j进行加加减减
    int i=l-1,j=r+1;
    //取中间值，若取左端点或者右端点在特殊情况下，算法会退化到O(n^2)，可能会卡数据
    int x=q[l+(r-l)/2];
    while(i<j){
        //先对i++,然后比较q[i]与x大小，如果q[i]<x,i继续往右边移动，直到不满足该条件为止。
        //j同理
        //最后交换q[i],q[j]，继续循环
        while(q[++i]<x);
        while(q[--j]>x);
        if(i<j) swap(q[i],q[j]);
    }
    //分治
    quick_sort(q,l,j);
    quick_sort(q,j+1,r);
}
```



## 归并排序

2020年8月7日添加

稳定排序

适用题目：

1. AcWing 787
2. AcWing 788

```c++
//需要开一个辅助数组 tmp[maxn],存放数据数据 q[maxn]，排好序的数据存放在q[maxn]中
void merge_sort(int q[],int l,int r){
    //如果当前区间没有值，或者只有一个值，那就返回
    if(l>=r) return;
    //取中点
    int mid=l+(r-l)/2;
    //分治处理
    merge_sort(q,l,mid);
    merge_sort(q,mid+1,r);
    //k遍历tmp数组
    //i从左半部分起点l开始遍历
    //j从右半部分mid+1开始遍历
    int k=0,i=l,j=mid+1;
    //如果不越界
    while(i<=mid&&j<=r){
        //i指向的值小于等于j指向的值 取走q[i]
        if(q[i]<=q[j]) tmp[k++]=q[i++];
        else tmp[k++]=q[j++];
    }
    //扫尾工作
    while(i<=mid) tmp[k++]=q[i++];
    while(j<=r) tmp[k++]=q[j++];
    //i遍历 [l,r],j遍历tmp[maxn]中的k个数据[0,k)
    for(int i=l,j=0;i<=r,j<k;i++,j++) q[i]=tmp[j];
}
```



## 二分

2020年8月8日

两种形式

一般策略，我们先把mid写成不加1的形式，然后如果l=mid，那就需要+1，如果r=mid，那就不需要+1；

取完mid之后，我们写一个check(mid)函数，来判断mid是否满足我们需要的条件。

适用题目：

1. AcWing 789 数的范围



```c++
//形式1
int l=0,r=n-1;
while(l<r){
    int mid=l+(r-l)/2;
    if(q[mid]>=x) r=mid;
    else l=mid+1;
}
//形式2
l=0,r=n-1;
while(l<r){
    int mid=l+(r-l+1)/2;
    if(q[mid]<=x) l=mid;
    else r=mid-1;
}
```

