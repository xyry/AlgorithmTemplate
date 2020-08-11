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



## 前缀和与差分

### AcWing 795. 前缀和

一维前缀和，注意数组下标从1开始，这样方便。

```c++
#include<iostream>
using namespace std;
const int maxn=1e5+10;
int q[maxn],pre[maxn],n,m;
int main(){
    scanf("%d%d",&n,&m);
    for(int i=1;i<=n;i++) scanf("%d",&q[i]);
    pre[0]=0;
    for(int i=1;i<=n;i++){
        pre[i]=pre[i-1]+q[i];
        // printf("pre[%d]=%d\n",i,pre[i]);
    }
    while(m--){
        int l,r;
        scanf("%d%d",&l,&r);
        printf("%d\n",pre[r]-pre[l-1]);
    }
    return 0;
}
```



### AcWing 796. 子矩阵的和

二维前缀和，注意画图理解。

```c++
#include<iostream>
using namespace std;
const int N=1010;
int n,m,q;
int a[N][N],s[N][N];
int main(){
    scanf("%d%d%d",&n,&m,&q);
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            scanf("%d",&a[i][j]);
        }
    }
    
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            s[i][j]=s[i-1][j]+s[i][j-1]-s[i-1][j-1]+a[i][j];
        }
    }
    
    while(q--){
        int x1,y1,x2,y2;
        scanf("%d%d%d%d",&x1,&y1,&x2,&y2);
        printf("%d\n",s[x2][y2]-s[x1-1][y2]-s[x2][y1-1]+s[x1-1][y1-1]);
    }
    return 0;
}
```



### AcWing 797. 差分

一维差分

题解：

```c++
#include<iostream>
using namespace std;
const int N=1e5+10;
int a[N],b[N];
int n,m;

void insert(int l,int r,int c){
    b[l]+=c;
    b[r+1]-=c;
}

int main(){
    scanf("%d%d",&n,&m);
    for(int i=1;i<=n;i++){
        scanf("%d",&a[i]);
    }
    for(int i=1;i<=n;i++) insert(i,i,a[i]);
    while(m--){
        int l,r,c;
        scanf("%d%d%d",&l,&r,&c);
        insert(l,r,c);
    }
    for(int i=1;i<=n;i++) a[i]=a[i-1]+b[i];
    for(int i=1;i<=n;i++) printf("%d ",a[i]);
    return 0;
}
```



### AcWing 798. 差分矩阵

二维差分，最后更新的时候注意是对bij进行求和，和求前缀和矩阵是反过来的。注意画图理解。

题解：

```c++
#include<iostream>

using namespace std;
const int N=1010;
int a[N][N],b[N][N];
int n,m,q;

void insert(int x1,int y1,int x2,int y2,int c){
    b[x1][y1]+=c;
    b[x1][y2+1]-=c;
    b[x2+1][y1]-=c;
    b[x2+1][y2+1]+=c;
}

int main(){
    scanf("%d%d%d",&n,&m,&q);
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            scanf("%d",&a[i][j]);
        }
    }
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            insert(i,j,i,j,a[i][j]);
        }
    }
    
    while(q--){
        int x1,y1,x2,y2,c;
        scanf("%d%d%d%d%d",&x1,&y1,&x2,&y2,&c);
        insert(x1,y1,x2,y2,c);
    }
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            b[i][j]+=b[i-1][j]+b[i][j-1]-b[i-1][j-1];
        }
    }
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            printf("%d ",b[i][j]);
        }
        puts("");
    }
    
    return 0;
}
```



## 双指针算法

2020年8月11日

### AcWing 799. 最长连续不重复子序列

模拟双指针

```c++
#include<iostream>
using namespace std;

const int N=1e5+10;
int n,res;
int a[N],s[N];

int main(){
    scanf("%d",&n);
    res=0;
    for(int i=0;i<n;i++) scanf("%d",&a[i]);
    for(int i=0,j=0;i<n;i++){
        s[a[i]]++;
        while(s[a[i]]>1){
            s[a[j]]--;
            j++;
        }
        res=max(res,i-j+1);
    }
    printf("%d",res);
    return 0;
}
```



### AcWing 800. 数组元素的目标和

裸题

```c++
#include<iostream>
using namespace std;
const int N=1e5+10;
int n,m,x;
int a[N],b[N];
int main(){
    scanf("%d%d%d",&n,&m,&x);
    for(int i=0;i<n;i++) scanf("%d",&a[i]);
    for(int i=0;i<m;i++) scanf("%d",&b[i]);
    int i=0,j=m-1;
    while(i<n&&j>=0){
        int cur=a[i]+b[j];
        if(cur==x){
            printf("%d %d\n",i,j);
            break;
        } 
        if(cur<x) i++;
        else j--;
    }
    return 0;
    
}
```

### AcWing 801. 二进制中1的个数

挺有意思的，使用了lowbit，求出x中最后一个1。

```c++
#include<iostream>
using namespace std;
const int N=1e5+10;
int a[N];
int n;

int lowbit(int n){
    return n&(-n);
}

int main(){
    scanf("%d",&n);
    for(int i=0;i<n;i++) scanf("%d",&a[i]);
    for(int i=0;i<n;i++){
        int x=a[i];
        int res=0;
        //如果x不为0，减去lowbit求出最后一个1的位置，不明白可以手推一下
        while(x) x-=lowbit(x),res++;
        printf("%d ",res);
    }
    return 0;
}
```

### AcWing 802. 区间合

思路：离散化，用二分找坐标，然后用前缀和处理。

```c++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
typedef pair<int,int> PII;

const int N=3e5+10;
int a[N],s[N];
int n,m;
vector<int> alls;
vector<PII> adds,query;

int find(int x){
    int l=0,r=alls.size()-1;
    while(l<r){
        int mid=l+(r-l)/2;
        if(alls[mid]>=x) r=mid;
        else l=mid+1;
    }
    return r+1;//1,2,3,...,n 为了方便处理前缀和，所以下标要从1开始
    
}

int main(){
    scanf("%d%d",&n,&m);
    for(int i=0;i<n;i++){
        int x,c;
        scanf("%d%d",&x,&c);
        alls.push_back(x);
        adds.push_back({x,c});
    }
    for(int i=0;i<m;i++){
        int l,r;
        scanf("%d%d",&l,&r);
        query.push_back({l,r});
        alls.push_back(l);
        alls.push_back(r);
    }
    
    sort(alls.begin(),alls.end());
    alls.erase(unique(alls.begin(),alls.end()),alls.end());
    for(auto item:adds){
        int x=find(item.first);
        a[x]+=item.second;
    }
    
    for(int i=1;i<=alls.size();i++){
        s[i]=s[i-1]+a[i];
    }
    
    for(auto item:query){
        int l=find(item.first);
        int r=find(item.second);
        printf("%d\n",s[r]-s[l-1]);
    }
    return 0;
}
```

### AcWing 803. 区间合并

思路，对**左端点排序**，然后操作，具体看代码

```c++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
typedef pair<int,int> PII;

int n;
vector<PII> segs;

void merge(vector<PII> &segs){
    vector<PII> res;
    sort(segs.begin(),segs.end());
    int st=-2e9,ed=-2e9;
    for(auto seg:segs){
        if(ed<seg.first){
            if(st!=-2e9) res.push_back({st,ed});
            st=seg.first,ed=seg.second;
        }else{
            ed=max(ed,seg.second);
        }
    }
    if(st!=-2e9) res.push_back({st,ed});
    segs=res;
}


int main(){
    scanf("%d",&n);
    for(int i=0;i<n;i++){
        int l,r;
        scanf("%d%d",&l,&r);
        segs.push_back({l,r});
    }
    merge(segs);
    printf("%ld\n",segs.size());
    return 0;
}
```



