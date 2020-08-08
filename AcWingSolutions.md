[TOC]

AcWing 做题记录

# 算法基础课

## 排序

### AcWing 785. 快速排序

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

### AcWing 786. 第k个数

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





### AcWing 787. 归并排序

模板：归并排序

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

题解：

```c++
#include<iostream>
using namespace std;
const int maxn=1e6+10;
int n;
int q[maxn],tmp[maxn];

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
    for(int i=l,j=0;i<=r,j<k;i++,j++){
        q[i]=tmp[j];
    }
}


int main(){
    scanf("%d",&n);
    for(int i=0;i<n;i++) scanf("%d",&q[i]);
    merge_sort(q,0,n-1);
    for(int i=0;i<n;i++) printf("%d ",q[i]);
    return 0;
}
```

### AcWing 788. 逆序对的数量

2020年8月8日

模板：归并排序

大体思路归并排序的时候，统计逆序对数量，等于左半部分的数量+右半部分的数量+左右共同构成的数量（mid-i+1）

```c++
#include<iostream>
using namespace std;
typedef long long ll;
const int maxn=1e5+10;

int n;
int q[maxn],tmp[maxn];

ll merge_sort(int l,int r){
    if(l>=r) return 0;
    int mid=l+(r-l)/2;
    ll res=merge_sort(l,mid)+merge_sort(mid+1,r);
    int k=0,i=l,j=mid+1;
    while(i<=mid&&j<=r){
        if(q[i]<=q[j]) tmp[k++]=q[i++];
        else{
            tmp[k++]=q[j++];
            res+=mid-i+1;
        }
    }
    while(i<=mid) tmp[k++]=q[i++];
    while(j<=r) tmp[k++]=q[j++];
    for(int i=l,j=0;i<=r;i++,j++) q[i]=tmp[j];
    return res;
}

int main(){
    scanf("%d",&n);
    for(int i=0;i<n;i++) scanf("%d",&q[i]);
    printf("%lld\n",merge_sort(0,n-1));
    return 0;
}
```

## 二分

### AcWing 789. 数的范围 

模板：二分模板

两种模板都用上了，注意什么时候有+1，什么时候没有。

```c++
#include<iostream>
using namespace std;
const int maxn=1e5+10;
int n,m;
int q[maxn];

int main(){
    scanf("%d%d",&n,&m);
    for(int i=0;i < n ; i ++) scanf("%d",&q[i]);
    while(m--){
        int x;
        scanf("%d",&x);
        int l=0,r=n-1;
        while(l<r){
            int mid=l+(r-l)/2;
            if(q[mid]>=x) r=mid;
            else l=mid+1;
        }
        if(q[l]!=x) printf("-1 -1\n");
        else{
            printf("%d ",l);
            l=0,r=n-1;
            while(l<r){
                int mid=l+(r-l+1)/2;
                if(q[mid]<=x) l=mid;
                else r=mid-1;
            }
            printf("%d\n",l);
        }
    }
    return 0;
}
```

