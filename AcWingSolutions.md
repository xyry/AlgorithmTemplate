[TOC]

AcWing 做题记录

## AcWing 786.第k个数

模板：快排

题解，快速选择法O(n)（快速排序O(nlogn)的变种）

```c++
#include<iostream>
using namespace std;
const int maxn=1e5+10;
int n,k;
int q[maxn];

int quick_sort(int l,int r,int k){
    if(l==r) return q[l];
    int i=l-1,j=r+1,x=q[l+(r-l)/2];
    while(i<j){
        while(q[++i]<x);
        while(q[--j]>x);
        if(i<j) swap(q[i],q[j]);
    }
    int sl=j-l+1;
    if(sl>=k) return quick_sort(l,j,k);
    else return quick_sort(j+1,r,k-sl);
}

int main(){
    scanf("%d %d",&n,&k);
    for(int i=0;i<n;i++) scanf("%d",&q[i]);
    printf("%d\n",quick_sort(0,n-1,k));
    return 0;
}
```



 ## AcWing 785.快速排序

模板：快排

手写一个快排，当作模板背过。时间复杂度O(nlogn)

```c++
#include<iostream>
using namespace std;
const int maxn=1e5+10;
int n;
int q[maxn];

void quick_sort(int q[],int l,int r){
    if(l>=r) return;
    int i=l-1,j=r+1,x=q[l+(r-l)/2];
    while(i<j){
        while(q[++i]<x);
        while(q[--j]>x);
        if(i<j) swap(q[i],q[j]);
    }
    quick_sort(q,l,j);
    quick_sort(q,j+1,r);
}

int main(){
    scanf("%d",&n);
    for(int i=0;i<n;i++) scanf("%d",&q[i]);
    quick_sort(q,0,n-1);
    for(int i=0;i<n;i++) printf("%d ",q[i]);
    return 0;
}
```



