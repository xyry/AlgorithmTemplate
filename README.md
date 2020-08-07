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
