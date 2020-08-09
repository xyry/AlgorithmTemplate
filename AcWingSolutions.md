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

复习次数：1

早上做的，晚上就忘，- - 2020年8月8日20点41分

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

### AcWing 790. 数的三次方根

模板：浮点数二分

需要注意的点，**double输入用lf 输出用f**

背过，如果背不过，使用cin>>x  

题解：

```c++
#include<iostream>
using namespace std;

int main(){
    double x;
    // float x;
    scanf("%lf",&x);
    double l=-10000,r=10000;
    while(r-l>1e-8){
        double m=l+(r-l)/2;
        if(m*m*m>=x) r=m;
        else l=m;
    }
    printf("%.6f\n",l);
    return 0;
}
//两个问题，为什么不能用float
```



## 高精度

2020年8月9日 下面4道题

### AcWing 791. 高精度加法

给定两个正整数，计算它们的和。

输入格式：共两行，每行包含一个整数。

输出格式：共一行，包含所求的和。

数据范围：1≤整数长度≤100000

模板：高精度加法

题解：

```c++
#include<iostream>
#include<vector>
using namespace std;

vector<int> add(vector<int> &A,vector<int> &B){
    vector<int> C;
    int t=0;
    if(A.size()<B.size()) return add(B,A);
    for(int i=0;i<A.size();i++){
        t+=A[i];
        if(i<B.size()) t+=B[i];
        C.push_back(t%10);
        t/=10;
    }
    if(t) C.push_back(1);
    return C;
}

int main(){
    string a,b;
    cin>>a>>b;
    vector<int> A,B;
    for(int i=a.size()-1;i>=0;i--) A.push_back(a[i]-'0');
    for(int i=b.size()-1;i>=0;i--) B.push_back(b[i]-'0');
    auto C=add(A,B);
    for(int i=C.size()-1;i>=0;i--) printf("%d",C[i]);
    return 0;
}
```

### Acwing 792. 高精度减法

给定两个正整数，计算它们的差，计算结果可能为负数。

输入格式：共两行，每行包含一个整数。

输出格式：共一行，包含所求的差。

数据范围：1≤整数长度≤105

模板：高精度减法

题解：

```c++
#include<iostream>
#include<vector>
using namespace std;

//比较AB大小 如果A大返回true
bool cmp(vector<int> &A,vector<int> &B){
    if(A.size()!=B.size()) return A.size()>B.size();
    else{
        for(int i=A.size()-1;i>=0;i--){
            if(A[i]!=B[i]){
                return A[i]>B[i];
            }
        }
    }
    return true;
}

vector<int> sub(vector<int> &A,vector<int> &B){
    vector<int> C;
    int t=0;
    for(int i=0;i<A.size();i++){
        t=A[i]-t;
        if(i<B.size()) t-=B[i];
        C.push_back((t+10)%10);
        if(t<0) t=1;
        else t=0;
    }
    while(C.size()>1&&C.back()==0) C.pop_back();
    return C;
}

int main(){
    string a,b;
    cin>>a>>b;
    vector<int> A,B;
    for(int i=a.size()-1;i>=0;i--) A.push_back(a[i]-'0');
    for(int i=b.size()-1;i>=0;i--) B.push_back(b[i]-'0');
    if(cmp(A,B)){
        auto C=sub(A,B);
        for(int i=C.size()-1;i>=0;i--) printf("%d",C[i]);
    }else{
        auto C=sub(B,A);
        printf("-");
        for(int i=C.size()-1;i>=0;i--) printf("%d",C[i]);
    }
    return 0;
}
```



### Acwing 793. 高精度乘法

给定两个正整数A和B，请你计算A * B的值。

输入格式：共两行，第一行包含整数A，第二行包含整数B。

输出格式：共一行，包含A * B的值。

数据范围：1≤A的长度≤1000001≤A的长度≤100000,0≤B≤10000

**高精度乘一个低精度**

模板：高精度乘法

题解：

```c++
#include<iostream>
#include<vector>
using namespace std;

vector<int> multi(vector<int> &A,int B){
    vector<int> C;
    int t=0;
    for(int i=0;i<A.size()||t;i++){
        if(i<A.size()) t+=A[i]*B;
        C.push_back(t%10);
        t/=10;
    }
    //去除前导零 WA
    while(C.size()>1&&C.back()==0) C.pop_back();
    return C;
}


int main(){
    string a;
    int B;
    cin>>a>>B;
    vector<int> A;
    for(int i=a.size()-1;i>=0;i--) A.push_back(a[i]-'0');
    auto C=multi(A,B);
    for(int i=C.size()-1;i>=0;i--) printf("%d",C[i]);
    
    return 0;
}
```



### Acwing 794. 高精度除法

给定两个非负整数A，B，请你计算 A / B的商和余数。

输入格式：共两行，第一行包含整数A，第二行包含整数B。

输出格式：共两行，第一行输出所求的商，第二行输出所求余数。

数据范围：1≤A的长度≤100000,1≤B≤10000，B 一定不为0

模板：高精度除法

题解：

```c++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

vector<int> div(vector<int> &A,int B,int &r){
    vector<int> C;
    r=0;
    for(int i=A.size()-1;i>=0;i--){
        r=r*10+A[i];
        C.push_back(r/B);
        r%=B;
    }
    reverse(C.begin(),C.end());
    while(C.size()>1&&C.back()==0) C.pop_back();
    return C;
}

int main(){
    string a;
    int B;
    //我自己都佛了,没输数据进来,怪不得输出为空
    cin>>a>>B;
    vector<int> A;
    for(int i=a.size()-1;i>=0;i--) A.push_back(a[i]-'0');
    int r;
    auto C=div(A,B,r);
    for(int i=C.size()-1;i>=0;i--) printf("%d",C[i]);
    printf("\n%d\n",r);
    return 0;
}
```

