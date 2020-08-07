[TOC]



# AlgorithmTemplate

记录做题用的模板,免得每次都现写

## 快速排序
日期:2020年08月07日10:36:47
适用题目

1. acwing 785
2. acwing 786

```c++
//复杂度O(nlogn)
void quick_sort(int q[],int l,int r){
    if(l>=r) return;
    int i=l-1,j=r+1;
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

适用题目：

1. AcWing 787

```c++
//需要开一个辅助数组 tmp[maxn],存放数据数据 q[maxn]，排好序的数据存放在q[maxn]中
void merge_sort(int q[],int l,int r){
    if(l>=r) return;
    int mid=l+(r-l)/2;
    merge_sort(q,l,mid);
    merge_sort(q,mid+1,r);
    int k=0,i=l,j=mid+1;
    while(i<=mid&&j<=r){
        if(q[i]<=q[j]) tmp[k++]=q[i++];
        else tmp[k++]=q[j++];
    }
    while(i<=mid) tmp[k++]=q[i++];
    while(j<=r) tmp[k++]=q[j++];
    //i遍历 [l,r],j遍历tmp[maxn]中的k个数据[0,k)
    for(int i=l,j=0;i<=r,j<k;i++,j++) q[i]=tmp[j];
}
```

