[TOC]

AcWing 做题记录

# 算法基础课

## 第一讲 基础算法

### 排序

#### AcWing 785. 快速排序

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

#### AcWing 786. 第k个数

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





#### AcWing 787. 归并排序

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

#### AcWing 788. 逆序对的数量

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

### 二分

#### AcWing 789. 数的范围 

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

#### AcWing 790. 数的三次方根

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



### 高精度

2020年8月9日 下面4道题

#### AcWing 791. 高精度加法

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

#### Acwing 792. 高精度减法

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



#### Acwing 793. 高精度乘法

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



#### Acwing 794. 高精度除法

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



### 前缀和与差分

#### AcWing 795. 前缀和

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



#### AcWing 796. 子矩阵的和

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



#### AcWing 797. 差分

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



#### AcWing 798. 差分矩阵

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



### 双指针算法

2020年8月11日

#### AcWing 799. 最长连续不重复子序列

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



#### AcWing 800. 数组元素的目标和

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

#### AcWing 801. 二进制中1的个数

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

#### AcWing 802. 区间合

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

#### AcWing 803. 区间合并

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

## 第二讲 数据结构

### 单链表

#### AcWing 826. 单链表

注意scanf()读字符，要记得吃回车，使用cin就不用考虑这么多，不说了，都是泪。

```c++
#include<iostream>
using namespace std;
const int N=1e5+10;
int m;

int head,idx;
int e[N],ne[N];

void init(){
    head=-1;
    idx=0;
}

void add_to_head(int x){
    e[idx]=x;
    ne[idx]=head;
    head=idx++;
}

void add(int k,int x){
    e[idx]=x;
    ne[idx]=ne[k];
    ne[k]=idx++;
    
}

void remove(int k){
    ne[k]=ne[ne[k]];
}



int main(){
    // scanf("%d\n",&m);
    cin>>m;
    //没吃回车，数据输入都读不完
    //太艹了，cin就会吃掉回车，scanf要手动吃回车
    init();
    while(m--){
        char op;
        int k,x;
        cin>>op;
        // scanf("%c",&op);
        if(op=='H'){
            cin>>x;
            // scanf("%d\n",&x);
            // cout<<op<<" "<<x<<endl;
            add_to_head(x);
        }else if(op=='D'){
            cin>>k;
            // scanf("%d\n",&k);
            // cout<<op<<" "<<k<<endl;
            if(!k) head=ne[head];
            else remove(k-1);
        }else{
            cin>>k>>x;
            // scanf("%d %d\n",&k,&x);
            // cout<<op<<" "<<k<<" "<<x<<endl;
            add(k-1,x);
        }
        // cout<<op<<" ";
        // for(int i=head;i!=-1;i=ne[i]) printf("%d ",e[i]);
        // puts("");
    }
    for(int i=head;i!=-1;i=ne[i]) printf("%d ",e[i]);
    puts("");
    return 0;
}
```

### 双链表

我们令 0为左端点 1为右端点，具体看代码，

注意idx++ TLE 30分钟

```c++
#include<iostream>
#include<string>
using namespace std;
const int N=1e5+10;
int e[N],l[N],r[N];
int m,idx;

void init(){
    r[0]=1,l[1]=0;
    idx=2;
}

void add(int k,int x){
    e[idx]=x;
    l[idx]=k;
    r[idx]=r[k];
    l[r[k]]=idx;
    r[k]=idx;
    idx++;
}

void remove(int k){
    r[l[k]]=r[k];
    l[r[k]]=l[k];
}

int main(){
    cin>>m;
    init();
    while(m--){
        string op;
        int x,k;
        cin>>op;
        if(!op.compare("R")){
            cin>>x;
            // cout<<"R1"<<endl;
            add(l[1],x);
        }else if(!op.compare("L")){
            cin>>x;
            // cout<<"L1"<<endl;
            add(0,x);
        }else if(!op.compare("D")){
            cin>>k;
            // cout<<"D1"<<endl;
            remove(k+1);
        }else if(!op.compare("IL")){
            cin>>k>>x;
            // cout<<"IL"<<k<<x<<endl;
            add(l[k+1],x);
        }else if(!op.compare("IR")){
            cin>>k>>x;
            // cout<<"IR"<<k<<x<<endl;
            add(k+1,x);
        }
        // cout<<op<<" ";
        // for(int i=0;i!=1;i=l[i]) cout<<e[i]<<" ";
    }
    for(int i=r[0];i!=1;i=r[i]) cout<<e[i]<<" ";
    puts("");
    return 0;
}
```

### 栈

模拟栈

```c++
#include<iostream>
using namespace std;

const int N=1e5+10;

int stack[N];
int m,top;

void init(){
    top=-1;
}

void push(int x){
    stack[++top]=x;
}
void pop(){
    top--;
}

bool empty(){
    if(top!=-1) return true;
    else return false;
}

int query(){
    return stack[top];
}

int main(){
    init();
    cin>>m;
    string op;
    int x;
    while(m--){
        cin>>op;
        if(!op.compare("push")){
            cin>>x;
            push(x);
        }else if(!op.compare("pop")){
            pop();
        }else if(!op.compare("empty")){
            if(empty()){
                cout<<"NO"<<endl;
            }else{
                cout<<"YES"<<endl;
            }
        }else{
            cout<<query()<<endl;
        }
    }
    return 0;
}
```



### 队列

#### AcWing 829. 模拟队列

直接模拟

```c++
#include<iostream>
using namespace std;
const int N=1e5+10;
int m;
int q[N],hh,tt;

void init(){
    hh=0;
    tt=-1;
}

void push(int x){
    q[++tt]=x;
}

void pop(){
    hh++;
}

bool empty(){
    if(tt>=hh) return true;
    else return false;
}

int main(){
    cin>>m;
    init();
    while(m--){
        string op;
        int x;
        cin>>op;
        if(op=="push"){
            cin>>x;
            push(x);
        }else if(op=="pop"){
            pop();
        }else if(op=="empty"){
            if(!empty()) cout<<"YES"<<endl;
            else cout<<"NO"<<endl;
        }else{
            cout<<q[hh]<<endl;
        }
    }
    return 0;
}
```

### 单调栈

#### AcWing 830. 单调栈

如果栈顶元素比其后面的元素小，那么后面的元素在后面的处理中永远不会被用到，所以做的时候处理掉，不断地更新栈。

```c++
#include<iostream>

using namespace std;

const int N=1e5+10;
int a[N],stk[N];
int top,n;

int main(){
    scanf("%d",&n);
    top=-1;
    for(int i=0;i<n;i++) scanf("%d",&a[i]);
    for(int i=0;i<n;i++){
        if(top==-1){
            cout<<"-1 ";
        }else if(a[i]>stk[top]){
            cout<<stk[top]<<" ";
        }else if(a[i]<=stk[top]){
            while(a[i]<=stk[top]&&top>-1){
                top--;
            }
            if(top==-1) cout<<"-1 ";
            else{
                cout<<stk[top]<<" ";
            }
        }
        stk[++top]=a[i];
    }
    return 0;
}

```

### 单调队列

#### AcWing 154. 滑动窗口

这题有点难写，单调栈的思路

```c++
#include<iostream>
#include<vector>
using namespace std;
const int N=1e6+10;
int n,k;
int a[N],q[N],hh,tt;

int main(){
    scanf("%d%d",&n,&k);
    for(int i=0;i<n;i++) scanf("%d",&a[i]);
    hh=0,tt=-1;
    for(int i=0;i<n;i++){
        //也就是q[hh]对应的坐标在窗口内，不在的话hh右移
        while(hh<=tt&&q[hh]<i-k+1) hh++;
        while(hh<=tt&&a[q[tt]]>=a[i]) tt--;
        q[++tt]=i;
        if(i>=k-1) printf("%d ",a[q[hh]]);
    }
    puts("");
    hh=0,tt=-1;
    for(int i=0;i<n;i++){
        while(hh<=tt&&q[hh]<i-k+1) hh++;
        while(hh<=tt&&a[q[tt]]<=a[i]) tt--;
        q[++tt]=i;
        if(i>=k-1) printf("%d ",a[q[hh]]);
    }
    
    return 0;
}
```

### KMP

#### AcWing 831. KMP字符串

KMP算法，说实话，看了很多遍，意思懂了，代码不太明白

```c++
#include<iostream>
using namespace std;
const int N=1e5+10;
const int M=1e6+10;
char p[N],s[M];
int n,m;
int ne[N];
int main(){
    //字符串下标从1开始存
    cin>>n>>p + 1>>m>>s + 1;
    for(int i=2,j=0;i<=n;i++){
        while(j&&p[i]!=p[j+1]) j=ne[j];
        if(p[i]==p[j+1]) j++;
        ne[i]=j;
    }
    
    for(int i=0,j=0;i<=m;i++){
        while(j&&s[i]!=p[j+1]) j=ne[j];
        if(s[i]==p[j+1]) j++;
        if(j==n){
            cout<<i-n<<" ";
            j=ne[j];
        }
    }
    return 0;
}
```

### Trie

2020年8月14日

#### AcWing 835. Trie字符串统计

模板题

```c++
#include<iostream>
using namespace std;
const int N=1e5+10;
int idx=0;
int son[N][26],cnt[N];


void insert(char str[]){
    int p=0;
    for(int i=0;str[i];i++){
        int u=str[i]-'a';
        if(!son[p][u]) son[p][u]=++idx;
        p=son[p][u];
    }
    cnt[p]++;
}
int query(char str[]){
    int p=0;
    for(int i=0;str[i];i++){
        int u=str[i]-'a';
        if(!son[p][u]) return 0;
        p=son[p][u];
    }
    return cnt[p];
}


int main(){
    int n;
    scanf("%d",&n);
    for(int i=0;i<n;i++){
        char op[2];
        char str[N];
        scanf("%s%s",op,str);
        // printf("%s %s\n",op,str);
        if(op[0]=='I') insert(str);
        else printf("%d\n",query(str));
    }
    return 0;
}
```



#### AcWing 143. 最大异或对

模板题

```c++
#include<iostream>
#include<algorithm>
using namespace std;

const int M=3e7+10;
const int N=1e5+10;
int son[M][2],idx;
int a[N];

void insert(int x){
    int p=0;
    for(int i=30;~i;i--){
        int s=(x>>i)&1;
        if(!son[p][s]) son[p][s]=++idx;
        p=son[p][s];
    }
}

int query(int x){
    int p=0;
    int res=0;
    for(int i=30;~i;i--){
        int s=(x>>i)&1;
        if(son[p][!s]){
            res+=1<<i;
            p=son[p][!s];
        }else p=son[p][s];
    }
    return res;
}

int main(){
    int n;
    scanf("%d",&n);
    for(int i=0;i<n;i++){
        scanf("%d",&a[i]);
        insert(a[i]);
    }
    int res=0;
    for(int i=0;i<n;i++){
        res=max(res,query(a[i]));
    }
    printf("%d\n",res);
    return 0;
}
```



### 并查集

#### AcWing 836. 合并集合

并查集模板题

```c++
#include<iostream>
using namespace std;

const int N=1e5+10;
int n,m;
int p[N];

int find(int x){
    if(p[x]!=x) p[x]=find(p[x]);
    return p[x];
}


int main(){
    scanf("%d%d",&n,&m);
    for(int i=1;i<=n;i++) p[i]=i;
    while(m--){
        char op[2];
        int a,b;
        scanf("%s%d%d",op,&a,&b);
        if(op[0]=='M') p[find(a)]=find(b);
        else{
            if(find(a)==find(b)) printf("Yes\n");
            else printf("No\n");
        }
    }
    
    return 0;
}
```



#### AcWing 837. 连通块中点的数量

注意维护一个s数组，存放以i为根节点的点的数量，注意特判，如果c a a，自己连接自己的时候，s数组不用更新。

```c++
#include<iostream>
using namespace std;

const int N=1e5+10;

int n,m;
//维护一个s数组，只对根节点有效
int p[N],s[N];

int find(int x){
    if(p[x]!=x) p[x]=find(p[x]);
    return p[x];
}

int main(){
    scanf("%d%d",&n,&m);
    for(int i=1;i<=n;i++){
        p[i]=i;
        s[i]=1;  
    } 
    while(m--){
        char op[3];
        int a,b;
        scanf("%s",op);
        if(op[0]=='C'){
            scanf("%d%d",&a,&b);
            //防止自己加自己，要特判一下
            if(find(a)!=find(b))
                s[find(b)]+=s[find(a)];
            p[find(a)]=find(b);
        }else if(op[1]=='1'){
            scanf("%d%d",&a,&b);
            if(find(a)==find(b)) printf("Yes\n");
            else printf("No\n");
        }else{
            scanf("%d",&a);
            printf("%d\n",s[find(a)]);
            // int x=find(a);
            // int cnt=0;
            // for(int i=1;i<=n;i++){
            //     if(find(i)==x) cnt++;
            // }
            // printf("%d\n",cnt);
        }
    }
}
```

#### AcWing 240. 食物链

算是我目前为止，见过最难的题目了，自己一个人是绝对想不出来怎么解的



```c++
#include<iostream>
using namespace std;

const int N=1e5+10;
int p[N],d[N];

int find(int x){
    if(p[x]!=x){
        //这个操作，可以让p[x]指向p[x]所属集和的根节点
        int t=find(p[x]);
        //那么这个操作就是 x->p[x]的距离d[x]，加上p[x]->根节点的距离
        d[x]+=d[p[x]];
        //最后更新x的父节点为所属集合的根节点
        p[x]=t;
    }
    return p[x];
}

//d[x] x到x的父节点的距离，距离模3，余0（与根节点同类），余1（吃根节点），余2（被根节点吃）
int main(){
    int n,k;
    scanf("%d%d",&n,&k);
    for(int i=1;i<=n;i++) p[i]=i;
    int res=0;
    while(k--){
        int op;
        int x,y;
        scanf("%d%d%d",&op,&x,&y);
        if(x>n||y>n){
            res++;
        }else{
           int px=find(x),py=find(y);
           if(op==1){
               //(d[x]-d[y])%3!=0 意味着x,y不是同类
               if(px==py&&(d[x]-d[y])%3) res++;
               else if(px!=py){
                   p[px]=py;
                   //把x,y合并为同一类，那么(d[x]+?-d[y])%3==0
                   //解得 d[px]=d[y]-d[x]
                   d[px]=d[y]-d[x];//有可能为负数
               }
           }else{
               //不满足(d[x]-d[y]-1)%3!=0 x不可能吃y
               if(px==py&&(d[x]-d[y]-1)%3) res++;
               else if(px!=py){
                   p[px]=py;
                   //同理，需要满足x吃y,(d[x]+?-d[y]-1)%3==0
                   //d[px]=d[y]-d[x]+1
                   d[px]=d[y]-d[x]+1;
               }
           }
        }
    }
    printf("%d\n",res);
    return 0;
}
```



### 堆

#### AcWing 838. 堆排序

模板题

```c++
#include<iostream>
using namespace std;
const int N=1e5+10;
int h[N],s;

void down(int u){
    int t=u;
    if(u*2<=s&&h[u*2]<h[t]) t=u*2;
    if(u*2+1<=s&&h[u*2+1]<h[t]) t=u*2+1;
    if(t!=u){
        swap(h[u],h[t]);
        down(t);
    }
}

int main(){
    int n,m;
    
    scanf("%d%d",&n,&m);
    s=n;
    for(int i=1;i<=n;i++) scanf("%d",&h[i]);
    
    for(int i=n/2;i>=1;i--) down(i);
    while(m--){
        printf("%d ",h[1]);
        h[1]=h[s];
        s--;
        down(1);
    }
    return 0;
    
    
}
```



#### AcWing 839. 模拟堆

我觉得这是一个比食物链还要难的题目。

就是索引指来指去很烦。

```c++
#include<iostream>
#include<string.h>
using namespace std;
const int N=1e5+10;
int h[N],ph[N],hp[N],s;

void heap_swap(int a,int b){
    swap(ph[hp[a]],ph[hp[b]]);
    swap(hp[a],hp[b]);
    swap(h[a],h[b]);
    
}


void down(int u){
    int t=u;
    if(2*u<=s&&h[2*u]<h[t]) t=2*u;
    if(2*u+1<=s&&h[2*u+1]<h[t]) t=2*u+1;
    if(t!=u){
        heap_swap(t,u);
        down(t);
    }
    
    
}

void up(int u){
    int t=u;
    if(u/2&&h[u/2]>h[t]) t=u/2;
    if(t!=u){
        heap_swap(t,u);
        up(t);
    }
}

int main(){
    int n,m=0;
    scanf("%d",&n);
    while(n--){
        char op[10];
        int k,x;
        scanf("%s",op);
        if(!strcmp(op,"I")){
            scanf("%d",&x);
            s++;
            m++;
            ph[m]=s;
            hp[s]=m;
            h[s]=x;
            up(s);
        }else if(!strcmp(op,"PM")){
            printf("%d\n",h[1]);
        }else if(!strcmp(op,"DM")){
            heap_swap(1,s);
            s--;    
            down(1);
        
        }else if(!strcmp(op,"D")){
            scanf("%d",&x);
            k=ph[x];
            heap_swap(k,s);
            s--;
            down(k),up(k);
        }else{
            scanf("%d%d",&k,&x);
            k=ph[k];
            h[k]=x;
            down(k),up(k);
        }
    }
    return 0;
}
```



### 哈希表

2020年8月18日

#### AcWing 840. 模拟散列表

拉链法解决冲突

```c++
#include<iostream>
#include<cstring>
using namespace std;

const int N=100003;
int h[N],e[N],ne[N],idx;

//拉链法，用一条单链表存储映射到同一个值k的数x
void insert(int x){
    //x可能为负数，负数取模后还是负数，所以要+N，在对N取模
    int k=(x%N+N)%N;
    //单链表头插法操作
    e[idx]=x;
    ne[idx]=h[k];
    h[k]=idx++;
}

bool find(int x){
    int k=(x%N+N)%N;
    for(int i=h[k];i!=-1;i=ne[i]){
        if(e[i]==x) return true;
    }
    return false;
}

int main(){
    
    int n;
    scanf("%d",&n);
    //单链表为空时，头指针指向-1
    memset(h,-1,sizeof(h));
    while(n--){
        char op[2];
        int x;
        scanf("%s%d",op,&x);
        if(op[0]=='I'){
            insert(x);
        }else{
            if(find(x)){
                printf("Yes\n");
            }else printf("No\n");
        }
    }
    
    return 0;
}
```

开放选址法

好处是只用开一个数组

```c++
#include<iostream>
#include<cstring>
using namespace std;

const int N=200003,null=0x3f3f3f3f;
int h[N];
//开放寻址法

//找到在哪里放x 
int find(int x){
    int k=(x%N+N)%N;
    while(h[k]!=null&&h[k]!=x){
        k++;
        if(k==N) k=0;
    }
    return k;    
}

int main(){
    int n;
    memset(h,null,sizeof(h));
    scanf("%d",&n);
    while(n--){
        char op[2];
        int x;
        scanf("%s%d",op,&x);
        int k=find(x);
        if(op[0]=='I'){
            h[k]=x;
        }else{
           if(h[k]!=null) printf("Yes\n");
           else printf("No\n");
        }
    }
    return 0;
}

```

#### AcWing 841. 字符串哈希

字符串哈希模板



```c++
#include<iostream>
typedef unsigned long long ULL;
using namespace std;

const int N=1e5+10,P=131;
int n,m;
char str[N];
ULL h[N],p[N];

ULL get(int l,int r){
    return h[r]-h[l-1]*p[r-l+1];
}

int main(){
    //为了让字符串下标从1开始
    scanf("%d%d%s",&n,&m,str+1);
    //预处理p数组，同时处理h数组
    //str[i]只要不让字符等于0即可
    p[0]=1;
    for(int i=1;i<=n;i++){
        p[i]=p[i-1]*P;
        h[i]=h[i-1]*P+str[i];
    }
    
    
    while(m--){
        int l1,r1,l2,r2;
        scanf("%d%d%d%d",&l1,&r1,&l2,&r2);
        if(get(l1,r1)==get(l2,r2)) puts("Yes");
        else puts("No");
        
    }
    return 0;
}
```

## 第三讲 搜索与图论

### DFS

#### AcWing 842. 排列数字

```c++
#include<iostream>
using namespace std;

const int N=10;
int path[N];
bool vis[N];
int n;
void dfs(int u){
    if(u==n){
        for(int i=0;i<n;i++) printf("%d ",path[i]);
        puts("");
        return;
    }
    for(int i=1;i<=n;i++){
        if(!vis[i]){
            path[u]=i;
            vis[i]=true;
            dfs(u+1);
            vis[i]=false;
        }
    }
}


int main(){
    scanf("%d",&n);
    dfs(0);
    return 0;
}
```

#### AcWing 843. n-皇后问题

我遇到的问题就是，如何判断对角线上的值

```c++
#include<iostream>
using namespace std;

//因为要存对角线的元素，所以N设置为2倍大小
const int N=20;
char g[N][N];
//dg[N] x+y  对角线上的值相同，手算一下就明白
//dg[N] x-y+n

bool row[N],col[N],dg[N],udg[N];
int n;

void dfs(int u){
    if(u==n){
        for(int i=0;i<n;i++) puts(g[i]);
        puts("");
        return;
    }
    
    for(int i=0;i<n;i++){
        if(!col[i]&&!dg[u+i]&&!udg[u-i+n]){
            g[u][i]='Q';
            row[u]=true;
            col[i]=true;
            dg[u+i]=true;
            udg[u-i+n]=true;
            dfs(u+1);
            g[u][i]='.';
            row[u]=false;
            col[i]=false;
            dg[u+i]=false;
            udg[u-i+n]=false;
        }
    }
}

int main(){
    scanf("%d",&n);
    for(int i=0;i<n;i++){
        for(int j=0;j<n;j++){
            g[i][j]='.';
        }
    }
    dfs(0);
    return 0;
}
```

### BFS

#### AcWing 844. 走迷宫

典型BFS裸题

```c++
#include<iostream>
#include<queue>

using namespace std;

const int N=110;
int g[N][N];
int n,m;
bool vis[N][N];
struct node{
    int x,y;
    int cnt;
};
int d[4][2]={{0,-1},{0,1},{-1,0},{1,0}};

int main(){
    scanf("%d%d",&n,&m);
    for(int i=0;i<n;i++){
        for(int j=0;j<m;j++){
            scanf("%d",&g[i][j]);
        }
    }
    node start;
    start.x=0;
    start.y=0;
    start.cnt=0;
    vis[0][0]=1;
    queue<node> q;
    q.push(start);
    while(!q.empty()){
        node cur=q.front();
        q.pop();
        int cx=cur.x;
        int cy=cur.y;
        int cn=cur.cnt;
        // cout<<cx<<","<<cy<<","<<cn<<endl;
        if(cx==n-1&&cy==m-1){
            printf("%d\n",cn);
            break;
        } 
        node t;
        for(int i=0;i<4;i++){
            t.x=cx+d[i][0];
            t.y=cy+d[i][1];
            t.cnt=cn+1;
            if(t.x>=0&&t.x<n&&t.y>=0&&t.y<m&&g[t.x][t.y]==0&&!vis[t.x][t.y]){
                vis[t.x][t.y]=1;
                q.push(t);
            }
        }
    }
    
    
    
    return 0;
}
```

#### AcWing 845. 八数码

难点，用字符串存储状态，然后模拟x与周围数字交换即可，

```c++
#include<iostream>
#include<queue>
#include<unordered_map>
using namespace std;

int d[4][2]={{0,-1},{0,1},{1,0},{-1,0}};

int bfs(string start){
    queue<string> q;
    string end="12345678x";
    unordered_map<string,int> dis;
    dis[start]=0;
    q.push(start);
    while(q.size()){
        string cur=q.front();q.pop();
        int dc=dis[cur];
        if(cur==end) return dc;
        int k=cur.find('x');
        int x=k/3,y=k%3;
        for(int i=0;i<4;i++){
            int nx=x+d[i][0];
            int ny=y+d[i][1];
            if(nx>=0&&nx<3&&ny>=0&&ny<3){
                swap(cur[k],cur[nx*3+ny]);
                if(!dis.count(cur)){
                    dis[cur]=dc+1;
                    q.push(cur);
                }
                swap(cur[k],cur[nx*3+ny]);
            }
        }
        
    }
    return -1;
}

int main(){
    string start;
    for(int i=0;i<9;i++){
        char c;
        cin>>c;
        start+=c;
    }
    // cout<<start;
    cout<<bfs(start)<<endl;
    return 0;
}
```

### 树与图的深度优先遍历

#### AcWing 846. 树的重心

需要理解一点

这里是枚举删除每个点，然后计算删除每个点之后，剩余各个连通块中的节点数量，连通块有两种：各个子树，父节点所在的连通块，各个子树中的节点数量，就是子节点dfs的返回值；父节点所在连通块中点的数量，就是总数减去当前整棵子树的节点数。搞明白这一点就容易理解这道题目的做法了。

```c++
#include<iostream>
#include<string.h>
using namespace std;
const int N=1e5+10,M=2*N;
int h[N],e[M],ne[M],idx;
bool vis[N];
int ans=N;

int n;

void add(int a,int b){
    e[idx]=b;
    ne[idx]=h[a];
    h[a]=idx++;
}

int dfs(int u){
    int res=0;
    int sum=1;
    vis[u]=1;
    for(int i=h[u];i!=-1;i=ne[i]){
        //注意记得用e[]数组把图中的点解出来，不然看半天都不知道哪里写错了，因为e[]索引是idx，每个点的唯一标识，e[idx]才是图中的某个节点值
        int j=e[i];
        if(!vis[j]){
            int s=dfs(j);
            res=max(res,s);
            sum+=s;
        }
    }
    //n-sum其实就是父节点所在连通块的点数，为什么呢，因为从父节点下来，父节点的vis[j]是打上了标记的，所以sum是不包含父节点所在连通块的点数的，所以这样做是正确的。
    res=max(res,n-sum);
    ans=min(res,ans);
    return sum;
}

int main(){
    scanf("%d",&n);
    memset(h,-1,sizeof(h));
    for(int i=0;i<n-1;i++){
        int a,b;
        scanf("%d%d",&a,&b);
        add(a,b);
        add(b,a);
    }
    dfs(1);
    printf("%d\n",ans);
    return 0;
}
```

#### AcWing 847. 图中点的层次

不得不说，我最拿手的还是BFS，深搜对我来说很费劲，但是BFS贼溜

```c++
#include<string.h>
#include<iostream>
#include<queue>
using namespace std;
typedef pair<int,int> PII;
const int N=1e5+10,M=2*N;
int h[N],e[M],ne[M],idx;
bool vis[N];
int n,m;

void add(int a,int b){
    e[idx]=b;
    ne[idx]=h[a];
    h[a]=idx++;
}

int main(){
    scanf("%d%d",&n,&m);
    memset(h,-1,sizeof(h));
    for(int i=0;i<m;i++){
        int a,b;
        scanf("%d%d",&a,&b);
        add(a,b);
    }
    queue<PII> q;
    vis[1]=1;
    q.push(make_pair(1,0));
    while(q.size()){
        PII cur=q.front();q.pop();
        int cid=cur.first;
        int cd=cur.second;
        if(cid==n){
            printf("%d\n",cd);
            return 0;
        }
        for(int i=h[cid];i!=-1;i=ne[i]){
            int j=e[i];
            if(!vis[j]){
                q.push(make_pair(j,cd+1));
                vis[j]=1;
            }
        }
    }
    printf("-1\n");
    return 0;
}
```

### 拓扑排序

#### AcWing 848. 有向图的拓扑序列

找入度为0的点，加入队列。

每次挑出一个点，就把该点连的点入度全部减去1。

```c++
#include<iostream>
#include<string.h>
const int N=1e5+10,M=2*N;
int h[N],e[M],ne[M],idx;
int rd[N],q[N];
using namespace std;
int n,m;
int hh,tt=-1;

void add(int a,int b){
    e[idx]=b;
    ne[idx]=h[a];
    h[a]=idx++;
}

bool topsort(){
    hh=0,tt=-1;
    for(int i=1;i<=n;i++){
        if(rd[i]==0){
            q[++tt]=i;
        }
    }
    while(hh<=tt){
        int cur=q[hh++];
        for(int i=h[cur];i!=-1;i=ne[i]){
            int j=e[i];
            rd[j]--;
            if(rd[j]==0) q[++tt]=j;
        }
    }
    return tt==n-1;
}

int main(){
    scanf("%d%d",&n,&m);
    memset(h,-1,sizeof(h));
    for(int i=0;i<m;i++){
        int a,b;
        scanf("%d%d",&a,&b);
        add(a,b);
        rd[b]++;
    }
    if(topsort()){
        for(int i=0;i<n;i++){
            printf("%d ",q[i]);
        }
        puts("");
    }else{
        printf("-1\n");
    }
    return 0;
}
```

### Dijkstra

#### AcWing 849. Dijkstra求最短路I

模板

```c++
#include<cstring>
#include<iostream>
#include<algorithm>
using namespace std;

//500 个点，1e5条边 稠密图，用邻接矩阵
const int N=510;
int g[N][N];
int dist[N];
bool st[N];
int n,m;

int dijkstra(){
    //初始化所有距离为正无穷
    memset(dist,0x3f,sizeof(dist));
    //起点到自己的距离为0
    dist[1]=0;
    
    //更新n次
    for(int i=0;i<n;i++){
        int t=-1;
        //找一个不在当前集合的，离起点最近的点，或者如果不存在离起点最近的点，就找一个没有访问过的点。
        for(int j=1;j<=n;j++){
            if(!st[j]&&(t==-1||dist[t]>dist[j]))
                t=j;
        }
        
        //打标记
        st[t]=1;
        for(int j=1;j<=n;j++){
            dist[j]=min(dist[j],dist[t]+g[t][j]);
        }
    }
    
    if(dist[n]==0x3f3f3f3f) return -1;
    else return dist[n];
    
    
}

int main(){
    scanf("%d%d",&n,&m);
    memset(g,0x3f,sizeof(g));
    for(int i=0;i<m;i++){
        int a,b,c;
        scanf("%d%d%d",&a,&b,&c);
        //去掉重边，取值最小的那条边
        g[a][b]=min(g[a][b],c);
    }
    int t=dijkstra();
    printf("%d\n",t);
    
    return 0;
}

```

#### AcWing 850. Dijkstra求最短路II

堆优化版本Dijkstra，时间复杂度 mlogm



```c++
#include<cstring>
#include<iostream>
#include<algorithm>
#include<queue>
using namespace std;
typedef pair<int,int> PII;
const int N=2e5+10;
//使用邻接表进行存储
int h[N],e[N],ne[N],w[N],idx;
int dist[N];
bool st[N];
int n,m;


void add(int a,int b,int c){
    e[idx]=b;
    //存储了一个权重
    w[idx]=c;
    ne[idx]=h[a];
    h[a]=idx++;
}

int dijkstra(){
    memset(dist,0x3f,sizeof(dist));
    //小根堆 从小到大排序，这种写法第一次见
    
    priority_queue<PII,vector<PII>,greater<PII>> heap;
    heap.push({0,1});
    while(heap.size()){
        PII cur=heap.top();heap.pop();
        int ver=cur.second;
        int d=cur.first;
        
        for(int i=h[ver];i!=-1;i=ne[i]){
            int j=e[i];
            //更新才往堆里添加元素，不然重复添加没有意义
            if(dist[j]>d+w[i]){
                dist[j]=d+w[i];
                heap.push({dist[j],j});
            }
        }
    }
    if(dist[n]==0x3f3f3f3f) return -1;
    return dist[n];
    
}

int main(){
    scanf("%d%d",&n,&m);
    memset(h,-1,sizeof(h));
    for(int i=0;i<m;i++){
        int a,b,c;
        scanf("%d%d%d",&a,&b,&c);
        add(a,b,c);
    }
    int t=dijkstra();
    printf("%d\n",t);
    return 0;
}
```

### Bellman_ford

n^2

#### AcWing 855. 有边数限制的最短路

```c++
#include<cstring>
#include<iostream>
#include<algorithm>
using namespace std;

const int N=510,M=1e4+10;
int n,m,k;
int dist[N],backup[N];
struct Edge{
    int a,b,w;
}edges[M];

int bellman_ford(){
    //距离数组初始化
    memset(dist,0x3f,sizeof dist);
    dist[1]=0;
    //更新k次，可以获得一条有k条边的最短路径。
    for(int i=0;i<k;i++){
        //为了防止串联更新，因为每一次只更新一条边
        memcpy(backup,dist,sizeof(dist));
        for(int j=0;j<m;j++){
            int a=edges[j].a;
            int b=edges[j].b;
            int w=edges[j].w;
            dist[b]=min(dist[b],backup[a]+w);
        }
    }
    //假设1-n没有路径，但是n-1 ->n 有一条-2的边权，那么dist[n]也会被更新成inf-2，所以要这样去判定1-n是否有路径
    //每一次边权绝对值10000，k最大500，所以dist[n]经历过负边权最小为inf-5e6 如果inf大于这个值，说明1->n没有路径
    if(dist[n]>0x3f3f3f3f/2) return -1;
    return dist[n];
}


int main(){
    scanf("%d%d%d",&n,&m,&k);
    for(int i=0;i<m;i++){
        int a,b,c;
        scanf("%d%d%d",&a,&b,&c);
        edges[i]={a,b,c};
    }
    int t=bellman_ford();
    if(t==-1) puts("impossible");
    else printf("%d\n",t);
    return 0;
}
```

### SPFA

和堆优化dijkstra特别相似

#### AcWing 851. spfa求最短路

```c++
#include<cstring>
#include<iostream>
#include<queue>

using namespace std;

const int N=1e5+10;
int h[N],e[N],ne[N],w[N],idx;
int dist[N];
bool st[N];
int n,m;

void add(int a,int b,int c){
    e[idx]=b;
    w[idx]=c;
    ne[idx]=h[a];
    h[a]=idx++;
}

int spfa(){
    //初始化
    memset(dist,0x3f,sizeof(dist));
    dist[1]=0;
    
    queue<int> q;
    q.push(1);
    st[1]=1;
    while(q.size()){
        int cur=q.front();
        q.pop();
        st[cur]=0;
        for(int i=h[cur];i!=-1;i=ne[i]){
            int j=e[i];
            if(dist[j]>dist[cur]+w[i]){
                dist[j]=dist[cur]+w[i];
                if(!st[j]){
                    q.push(j);
                    st[j]=1;
                }
            }
        }
        
    }
    
    //这种情况就直接判定等于了，因为在后面的点不会被更新到，和bellman-ford不一样
    if(dist[n]==0x3f3f3f3f) return -1;
    return dist[n];
}


int main(){
    scanf("%d%d",&n,&m);
    memset(h,-1,sizeof h);
    for(int i=0;i<m;i++){
        int a,b,c;
        scanf("%d%d%d",&a,&b,&c);
        add(a,b,c);
    }
    int t=spfa();
    if(t==-1) puts("impossible");
    else printf("%d\n",t);
    return 0;
}
```



#### AcWing 852. spfa判断负环



```c++
#include<cstring>
#include<iostream>
#include<queue>
using namespace std;

const int N=1e4+10;
int h[N],e[N],ne[N],idx,w[N];
bool st[N];
int dist[N],cnt[N];
int n,m;

void add(int a,int b,int c){
    e[idx]=b;
    w[idx]=c;
    ne[idx]=h[a];
    h[a]=idx++;
}

bool spfa(){
    //不需要初始化，因为我们不算最短距离，只判负环，遇到负权才会更新距离
    queue<int> q;
    //从1开始的路径可能没有负环，所以要把每个点都加到队列中，进行判断
    for(int i=1;i<=n;i++){
        st[i]=1;
        q.push(i);
    }
    while(q.size()){
        int cur=q.front();q.pop();
        st[cur]=0;
        for(int i=h[cur];i!=-1;i=ne[i]){
            int j=e[i];
            if(dist[j]>dist[cur]+w[i]){
                dist[j]=dist[cur]+w[i];
                cnt[j]=cnt[cur]+1;
                if(cnt[j]>=n) return true;
                if(!st[j]){
                    st[j]=1;
                    q.push(j);
                }
            }
        }
    }
    return false;
    
}

int main(){
    scanf("%d%d",&n,&m);
    //单链表必须要初始化h数组，不然死循环，TLE
    memset(h,-1,sizeof(h));
    for(int i=0;i<m;i++){
        int a,b,c;
        scanf("%d%d%d",&a,&b,&c);
        add(a,b,c);
    }
    if(spfa()) puts("Yes");
    else puts("No");
    return 0;
}

```

### Floyd

#### AcWing 854. Floyd求最短路

多源最短路径

```c++
#include<cstring>
#include<iostream>
#include<algorithm>
using namespace std;

const int N=210,INF=1e9;
int d[N][N];
int n,m,Q;

void floyd(){
    for(int k=1;k<=n;k++){
        for(int i=1;i<=n;i++){
            for(int j=1;j<=n;j++){
                d[i][j]=min(d[i][j],d[i][k]+d[k][j]);
            }
        }
    }
}


int main(){
    scanf("%d%d%d",&n,&m,&Q);
    for(int i=1;i<=n;i++){
        for(int j=1;j<=n;j++){
            if(i==j) d[i][j]=0;
            else d[i][j]=INF;
        }
    }
    for(int i=1;i<=m;i++){
        int a,b,w;
        scanf("%d%d%d",&a,&b,&w);
        //这样更新，可以去掉自环，和重边（取两个点之间最近的那一条边）
        d[a][b]=min(d[a][b],w);
    }
    floyd();
    while(Q--){
        int x,y;
        scanf("%d%d",&x,&y);
        //存在负权边，所以距离为inf也会被更新
        if(d[x][y]>INF/2) puts("impossible");
        else printf("%d\n",d[x][y]);
    }
    return 0;
}
```

### Prim

#### AcWing 858. Prim算法求最小生成树

模板题

```c++
#include<cstring>
#include<iostream>
#include<algorithm>

using namespace std;
const int N=510,INF=0x3f3f3f3f;
int g[N][N],dist[N];
bool st[N];
int n,m;
int res;

int prim(){
    memset(dist,0x3f,sizeof dist);
    
    for(int i=0;i<n;i++){
        int t=-1;
        for(int j=1;j<=n;j++){
            if(!st[j]&&(t==-1||dist[t]>dist[j])){
                t=j;
            }
        }
        st[t]=1;
        //不是第一个点，但是找到的离当前集合最小的点距离为INF，证明该图不连通
        if(i&&dist[t]==INF) return INF;
        //不是第一个点，找到离当前集合最近的距离，加上去，因为一个点没有边
        if(i) res+=dist[t];
        //重新更新集和外的点到集合里点的距离
        //与dijkstra算法的区别，dijkstra更新每个点到源点的距离，prim更新每个点到集合的最短距离。
        for(int j=1;j<=n;j++) dist[j]=min(dist[j],g[t][j]);
    
    }
}

int main(){
    scanf("%d%d",&n,&m);
    memset(g,INF,sizeof g);
    for(int i=0;i<m;i++){
        int a,b,c;
        scanf("%d%d%d",&a,&b,&c);
        //去掉重边
        g[a][b]=g[b][a]=min(g[a][b],c);
    }
    int t=prim();
    if(t==INF) puts("impossible");
    else printf("%d\n",res);
    
    return 0;
}
```

### Kruskal

#### AcWing 859. Kruskal 求最小生成树

```c++
#include<cstring>
#include<iostream>
#include<algorithm>
using namespace std;
const int N=2e5+10;
struct Edge{
    int a,b,w;
    bool operator < (const Edge& W)const{
        //从小排序，当前值比其他值小，排前面
        return w<W.w;
    }
}edges[N];
int p[N];
int n,m;

int find(int x){
    if(p[x]!=x) p[x]=find(p[x]);
    return p[x];
}

int main(){
    scanf("%d%d",&n,&m);
    for(int i=0;i<m;i++){
        int a,b,c;
        scanf("%d%d%d",&a,&b,&c);
        edges[i]={a,b,c};
    }
    //并查集的初始化
    for(int i=1;i<=n;i++) p[i]=i;
    //对所有边排序
    sort(edges,edges+m);
    int res=0;
    //加入边的个数
    int cnt=0;
    for(int i=0;i<m;i++){
        int a=edges[i].a,b=edges[i].b,w=edges[i].w;
        a=find(a);
        b=find(b);
        //a和b不在同一个连通块
        if(a!=b){
            //加入一条边，权重+w,次数+1，合并两个点
            res+=w;
            cnt++;
            p[b]=a;
        }
    }
    if(cnt<n-1) puts("impossible");
    else printf("%d\n",res);
    
    
    return 0;
}
```

###  染色法判定二分图

#### AcWing 860. 染色法判定二分图

```c++
#include<cstring>
#include<iostream>
#include<algorithm>
using namespace std;
const int N=1e5+10,M=2*N;
int n,m;
int h[N],e[M],ne[M],idx;
int color[N];

void add(int a,int b){
    e[idx]=b;
    ne[idx]=h[a];
    h[a]=idx++;
}

bool dfs(int u,int c){
    color[u]=c;
    for(int i=h[u];i!=-1;i=ne[i]){
        int j=e[i];
        if(!color[j]){
            dfs(j,3-c);
        }else if(color[j]==c) return false;
    }
    return true;
}

int main(){
    scanf("%d%d",&n,&m);
    memset(h,-1,sizeof h);
    for(int i=0;i<m;i++){
        int a,b;
        scanf("%d%d",&a,&b);
        add(a,b);
        add(b,a);
    }
    bool f=false;
    for(int i=1;i<=n;i++){
        if(!color[i]){
            if(!dfs(i,1)){
                puts("No");
                f=true;
                break;
            }
        }
    }
    if(!f) puts("Yes");
    
    return 0;
}
```

### 匈牙利算法

yxc讲的很有意思

#### AcWing 861. 二分图的最大匹配

```c++
#include<cstring>
#include<iostream>
#include<algorithm>

using namespace std;
//虽然是稠密图，但是我们只考虑集合1中的点，也就是只考虑a指向b，所以用邻接表存储是合适的。
const int N=510,M=1e5+10;
int n1,n2,m;
int h[N],e[M],ne[M],idx;
int match[N];
bool st[N];

void add(int a,int b){
    e[idx]=b;
    ne[idx]=h[a];
    h[a]=idx++;
}

bool find(int x){
    for(int i=h[x];i!=-1;i=ne[i]){
        int j=e[i];
        //如果当前点没有标记，这个标记不等于匹配，主要是为了递归找的时候用的
        if(!st[j]){
            //看上当前点
            st[j]=true;
            //如果右边点没有匹配，或者可以通过递归让其空出来
            if(match[j]==0||find(match[j])){
                match[j]=x;
                return true;
            }
        }
        
    }
    return false;
}

int main(){
    scanf("%d%d%d",&n1,&n2,&m);
    memset(h,-1,sizeof(h));
    for(int i=0;i<m;i++){
        int a,b;
        scanf("%d%d",&a,&b);
        add(a,b);
    }
    int res=0;
    for(int i=1;i<=n1;i++){
        //每个左边点都要把所其连接的右边点考虑一下，所以要全部置为false
        memset(st,false,sizeof(st));
        if(find(i)) res++;
    }
    printf("%d\n",res);
    return 0;
}
```

## 第四讲 数学知识

### 质数

#### AcWing 866. 试除法判定质数

就和方法名字一样，试除即可

```c++
#include<iostream>
using namespace std;
int n;

bool isprime(int x){
    if(x==1) return false; 
    for(int i=2;i*i<=x;i++){
        if(x%i==0) return false;
    }
    return true;
}

int main(){
    scanf("%d",&n);
    for(int i=0;i<n;i++){
        int x;
        scanf("%d",&x);
        if(isprime(x)){
            puts("Yes");
        }else puts("No");
        
    }
    return 0;
}
```

#### AcWing 867. 分解质因数

同样利用试除法，n为2的k次幂时，复杂度最好是$log(n)$，最坏是$\sqrt n$

```c++
#include<iostream>
using namespace std;

int n;

void divide(int n){
    for(int i=2;i<=n/i;i++){
        if(n%i==0){
            int s=0;
            while(n%i==0){
                n/=i;
                s++;
            }
            printf("%d %d\n",i,s);
        }
    }
    if(n>1) printf("%d %d\n",n,1);
    puts("");
}

int main(){
    scanf("%d",&n);
    for(int i=0;i<n;i++){
        int q;
        scanf("%d",&q);
        divide(q);
    }
    return 0;
}
```

#### AcWing 868. 筛质数

```c++
#include<iostream>
using namespace std;

const int N=1e6+10;
//false 为质数 true 为合数
int prime[N],cnt;
int n;
bool st[N];
//埃式筛法，只筛质数的倍数
//朴素筛法，筛掉所有数的倍数
//线性筛
int get_prime(int n){
   for(int i=2;i<=n;i++){
       if(!st[i]){
           prime[cnt++]=i;
       }
       for(int j=0;prime[j]<=n/i;j++){
           st[prime[j]*i]=1;
           if(i%prime[j]==0) break;// prime[j]是i的最小质因子的时候 退出循环
       }
   }
}

int main(){
    scanf("%d",&n);
    get_prime(n);
    printf("%d\n",cnt);
    return 0;
}
```

### 博弈论

#### AcWing 891. Nim游戏

```c++
#include<iostream>
using namespace std;
int n;
int main(){
    scanf("%d",&n);
    int res;
    for(int i=0;i<n;i++){
        int x;
        scanf("%d",&x);
        if(!i) res=x;
        else res^=x;
    }
    if(!res) puts("No");
    else puts("Yes");    
    return 0;
}
```

#### AcWing 892. 台阶-Nim博弈

我其实不太懂这是如何推导的

```c++
#include<iostream>
using namespace std;

int main(){
    int n;
    scanf("%d",&n);
    int res;
    for(int i=0;i<n;i++){
        int x;
        scanf("%d",&x);
        if(!i) res=x;
        else if(i%2==0){
            res^=x;
        }
    }
    if(res==0) puts("No");
    else puts("Yes");
    return 0;
}
```



#### AcWing 893. 集合-Nim博弈

SG函数 ，所有状态的SG函数异或为0，是必败状态



```c++
#include<iostream>
#include<cstring>
#include<unordered_set>
using namespace std;
const int N=110,M=1e4+10;
int f[M];
int a[N];
int n,k;

int sg(int x){
    if(f[x]!=-1) return f[x];
    unordered_set<int> S;
    for(int i=0;i<k;i++){
        if(x>=a[i]) S.insert(sg(x-a[i]));
    }
    //mex操作
    for(int i=0;;i++){
        if(!S.count(i)) return f[x]=i;
    }
}

int main(){
    cin>>k;
    for(int i=0;i<k;i++){
        cin>>a[i];
    }
    int res=0;
    memset(f,-1,sizeof f);
    cin>>n;
    for(int i=0;i<n;i++){
        int x;
        cin>>x;
        res^=sg(x);
    }
    if(res) puts("Yes");
    else puts("No");
    return 0;
}
```



#### AcWing 894. 拆分-Nim博弈

用到一个性质 sg(b1,b2)=sg(b1)^sg(b2);

```c++
#include<iostream>
#include<cstring>
#include<unordered_set>
using namespace std;
const int N=110;
int f[N];
int n;

int sg(int x){
    if(f[x]!=-1) return f[x];
    unordered_set<int> S;
    for(int i=0;i<x;i++){
        for(int j=0;j<=i;j++){
            S.insert(sg(i)^sg(j));
        }
    }
    //mex操作
    for(int i=0;;i++){
        if(!S.count(i)){
            return f[x]=i;
        }
    }
}

int main(){
    cin>>n;
    int res=0;
    //记忆化搜索初始化
    memset(f,-1,sizeof f);
    for(int i=0;i<n;i++){
        int x;
        cin>>x;
        res^=sg(x);
    }
    //SG值全部异或为0是必败局面
    if(res) puts("Yes");
    else puts("No");
    return 0;
}
```

## 第五讲 动态规划

### 背包问题

#### AcWing 2. 01背包问题

```c++
const int N=1100;
int v[N],w[N];
int f[N][N];

//n^2 做法
int main(){
    int n,V;
    cin>>n>>V;
    for(int i=1;i<=n;i++){
        cin>>v[i]>>w[i];
    }
    for(int i=1;i<=n;i++){
        for(int j=0;j<=V;j++){  
            f[i][j]=f[i-1][j];
            if(j>=v[i]){
                f[i][j]=max(f[i][j],f[i-1][j-v[i]]+w[i]);
            }
        }
    }
    int res=0;
    for(int i=1;i<=V;i++) res=max(res,f[n][i]);
    cout<<res<<endl;
    return 0;
}
*/
```

$O(V)$时间复杂度做法

```c++
#include<iostream>
using namespace std;
const int N=1010;
int f[N];
int v[N],w[N];
int main(){
    int n,m;
    cin>>n>>m;
    for(int i=1;i<=n;i++){
        cin>>v[i]>>w[i];
    }
    for(int i=1;i<=n;i++){
        for(int j=m;j>=v[i];j--){
            //倒着枚举 确保这个时候的f[j-v[i]]是上一个i-1的状态
            //如果正向枚举，这个时候的f[j-v[i]]其实是当前i的状态，自己更新自己的那种感觉，你明白吧。
            f[j]=max(f[j],f[j-v[i]]+w[i]);
        }
    }
    cout<<f[m]<<endl;
    return 0;
}
```

#### AcWing 3. 完全背包问题



```c++
//二维版本
#include<iostream>

using namespace std;
const int N=1010;
int f[N][N];
int v[N],w[N];
int n,m;

int main(){
    cin>>n>>m;
    for(int i=1;i<=n;i++){
        cin>>v[i]>>w[i];
    }
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            //第i个物品放0个的情况，一定存在， 所以直接更新一下
            f[i][j]=max(f[i][j],f[i-1][j]);
            if(j>=v[i]){
                //二维的情况下，是不需要考虑体积的遍历顺序的,因为i-1的状态是存在与f[i-1][j]里面的。
                f[i][j]=max(f[i-1][j],f[i][j-v[i]]+w[i]);
            }
        }
    }
    cout<<f[n][m]<<endl;
    return 0;
}
```

```c++
//一维优化版本
#include<iostream>

using namespace std;
const int N=1010;
int f[N];
int v[N],w[N];
int n,m;

int main(){
    cin>>n>>m;
    for(int i=1;i<=n;i++){
        cin>>v[i]>>w[i];
    }
    for(int i=1;i<=n;i++){
        for(int j=v[i];j<=m;j++){
            //这里的在算f[j]的时候，因为v[i]>0,所以j-v[i]<j，所以在更新f[j]的时候，f[j-v[i]]已经被算过了，所以f[j-v[i]]是第i个物品的状态
            //也就是f[i][j-v[i]] 正好符合
           f[j]=max(f[j],f[j-v[i]]+w[i]);
        }
    }
    cout<<f[m]<<endl;
    return 0;
}
```

#### AcWing 4. 多重背包问题I

我觉得很像完全背包，一个是物品数量无限，一个是物品数量有限。代码很好写

```c++
#include<iostream>
using namespace std;

const int N=110;
int f[N];
int main(){
    int n,m;
    cin>>n>>m;
    for(int i=0;i<n;i++){
        int v,w,s;
        cin>>v>>w>>s;
        for(int j=m;j>=0;j--){
            for(int k=1;k<=s&&k*v<=j;k++){
                //f[j]相当于不选当前物品,此时的f[j]是上一个物品的状态
                //
                f[j]=max(f[j],f[j-k*v]+k*w);
            }
        }
    }
    cout<<f[m]<<endl;
    return 0;
}
```

#### AcWing 5. 多重背包问题II

二进制优化版本，优化成01背包来做

```c++
#include<iostream>
#include<vector>

using namespace std;
const int M=2010;
int n,m;
int f[M];
struct Good{
    int v,w;
};

int main(){
    cin>>n>>m;
    vector<Good> goods;
    for(int i=0;i<n;i++){
        int v,w,s;
        cin>>v>>w>>s;
        //拆数
        for(int k=1;k<=s;k*=2){
            goods.push_back({v*k,w*k});
            s-=k;
        }
        if(s>0) goods.push_back({s*v,s*w});
    }
    
    for(auto good:goods){
        for(int j=m;j>=good.v;j--){
            f[j]=max(f[j],f[j-good.v]+good.w);
        }
    }
    cout<<f[m]<<endl;
    
    return 0;
}


```

#### AcWing 9. 分组背包问题

```c++
#include<iostream>
using namespace std;
const int N=110;

int f[N],v[N],w[N];
int n,m;
int main(){
    cin>>n>>m;
    for(int i=0;i<n;i++){
        int s;
        cin>>s;
        for(int j=0;j<s;j++){
            cin>>v[j]>>w[j];
        }
        for(int j=m;j>=0;j--){
            for(int k=0;k<s;k++){
                //判断一下能否装下
                if(j-v[k]>=0){
                    f[j]=max(f[j],f[j-v[k]]+w[k]);
                }
            }
        }
    }
    cout<<f[m]<<endl;
    return 0;
}
```

### 线性DP

#### AcWing 898. 数字三角形

最简单的

```c++
#include<iostream>
using namespace std;
const int N=550;
int a[N][N];
int f[N];
int n;

int main(){
    cin>>n;
    for(int i=1;i<=n;i++){
        for(int j=1;j<=i;j++){
            cin>>a[i][j];
        }
    }
    // for(int i=1;i<=n;i++){
    //     for(int j=1;j<=i;j++){
    //         cout<<a[i][j];
    //     }puts("");
    // }
    for(int i=1;i<=n;i++) f[i]=a[n][i];
    for(int i=n-1;i>=1;i--){
        for(int j=1;j<=i;j++){
            f[j]=a[i][j]+max(f[j],f[j+1]);
        }
    }
    cout<<f[1]<<endl;
    return 0;
}
```

#### AcWing 895. 最长上升子序列

$O(n^2)$的做法

状态数量n * 状态转移 n=n^2

动态规划

1. 状态表示 f[i]
   1. 集合：所有以第i个数结尾的上升子序列
   2. 属性：f[i]是一个值，这个值代表最大的子序列长度
2. 状态计算 $f[i]=max(f[j]+1),j=0,1,2,...,i-1$，所以我们需要遍历第i值前面所有值的f[j]，来找最大的那个值

```c++
#include<iostream>
using namespace std;

const int N=1010;
int f[N];
int n;
int a[N];
int main(){
    cin>>n;
    for(int i=0;i<n;i++){
        cin>>a[i];
        f[i]=1;
    }
    int res=0;
    for(int i=0;i<n;i++){
        for(int j=0;j<i;j++){
            if(a[i]>a[j]){
                f[i]=max(f[i],f[j]+1);
                res=max(res,f[i]);
            }
        }
    }
    cout<<res<<endl;
    return 0;
}
```

#### AcWing 896. 最长上升子序列II

$O(nlogn)$的做法

注释写的很详细

```c++
#include<iostream>
using namespace std;
const int N=1e5+10;
//q[i]存储长度为i的最长上升子序列结尾最小的值，可以证明长度越长，结尾最小的值是单调递增的。
int a[N],q[N];
int n;
int main(){
    scanf("%d",&n);
    for(int i=0;i<n;i++) scanf("%d",&a[i]);
    int len=0;
    q[0]=-2e9;
    for(int i=0;i<n;i++){
        //找到i之前小于a[i]的最大值，也就是找a[i]之前最长的上升子序列
        int l=0,r=len;
        while(l<r){
            int mid=l+r+1 >>1;
            if(q[mid]<a[i]) l=mid;
            else r=mid-1;
        }
        //a[i]前找到的最长上升子序列长度为l
        //那么就更新q[l+1]的值为 a[i]
        //如果q[l+1]的小于a[i],那么l+1一定会被找出来，不然q[l+1]的值一定大于a[i]，所以用a[i]更新q[l+1]是没问题的。
        q[++l]=a[i];
        len=max(len,l);
    }
    printf("%d\n",len);
    return 0;
}
```

#### AcWing 897. 最长公共子序列

代码比较简洁，但是推导挺麻烦

状态表示 $f[i][j]$

1. 集合：所有A,B字符串公共子序列的集合
2. 属性：集合中公共子序列长度的最大值

状态转移，一共四个状态

1. 选a[i],b[j]：$f[i][j]$
2. 不选a[i]，选b[j]：$f[i-1][j]$
3. 选a[i]，不选b[j]：$f[i][j-1]$
4. 不选a[i]，不选b[j]：$f[i-1][j-1]$

状态2中其实也包含了不选b[j]的状态，$f[i-1][j]$代表的是从a[1...i-1]和b[1...j]中选的公共子序列，一定不包含a[i],但不一定含b[j]，所以也是包含了不选b[j]的状态，也就是状态$f[i-1][j-1]$，所以状态2是包含了状态4的。状态三同理分析。

状态1很好理解，当a[i]==b[j]的时候，$f[i][j]=f[i-1][j-1]+1$

所以最后我们只需要考虑三个状态即可。

```c++
#include<iostream>
using namespace std;
const int N=1010;

int n,m;
char a[N],b[N];
int f[N][N];

int main(){
    cin>>n>>m>>a+1>>b+1;
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            f[i][j]=max(f[i-1][j],f[i][j-1]);
            if(a[i]==b[j]) f[i][j]=max(f[i][j],f[i-1][j-1]+1);
        }
    }
    cout<<f[n][m]<<endl;
    return 0;
}
```

#### AcWing 902. 最短编辑距离

```c++
#include<iostream>
#include<cstring>
using namespace std;
const int INF=0x3f3f3f3f;
const int N=1010;
char a[N],b[N];
int n,m;
int f[N][N];
int main(){
    cin>>n>>a+1>>m>>b+1;
    // memset(f,INF,sizeof f);
    
    //初始化，要单独看，当a用0个字符与b的i个字符匹配时，只能用添加操作
    for(int i=0;i<=m;i++) f[0][i]=i;
    
    //初始化，要单独看，当a用i个字符与b的0个字符匹配时，只能用删除操作
    for(int i=0;i<=n;i++) f[i][0]=i;
    
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            if(a[i]==b[j]){
                f[i][j]=f[i-1][j-1];
            }else {
                f[i][j]=min(f[i-1][j],min(f[i-1][j-1],f[i][j-1]))+1;
            }
        }
    }
    
    cout<<f[n][m]<<endl;
    return 0;
}
```

#### AcWing 899. 编辑距离

对每一对字符求最小编辑距离即可。

存在坑点，看代码

```c++
#include<iostream>
#include<cstring>
using namespace std;
//注意数据范围，不然会TLE.有1000个字符，每个字符长度为10，所以一维开1010，二维开15
const int N=15,M=1010;
int n,m;
char a[M][N],b[N];
int k;
bool check(char ta[],char tb[],int k){
    // cout<<"k="<<k<<endl;
    int f[N][N];
    memset(f,0,sizeof f);
    
    int sa=strlen(ta+1);
    int sb=strlen(tb+1);
    //当a取0个字符，b有i个字符，这个时候a->b  只能采用添加字符操作，操作次数为i次
    for(int j=0;j<=sb;j++) f[0][j]=j;
    //当a有i个字符，b有0个字符，这个时候a->b 只能采用删除操作，操作次数为i次
    for(int i=0;i<=sa;i++) f[i][0]=i;
        
    for(int i=1;i<=sa;i++){
        for(int j=1;j<=sb;j++ ){
            if(ta[i]==tb[j]) f[i][j]=f[i-1][j-1];
            else f[i][j]=min(f[i-1][j-1],min(f[i-1][j],f[i][j-1]))+1;
        }
    }
    // cout<<ta<<","<<tb<<","<<f[sa][sb]<<endl;
    if(f[sa][sb]<=k) return true;
    else return false;
}

int main(){
    cin>>n>>m;
    for(int i=1;i<=n;i++){
        cin>>a[i]+1;
    }
    for(int i=1;i<=m;i++){
        int ans=0;
        cin>>b+1>>k;
        // cout<<k<<endl;
        //在线做，输入一个处理一个
        for(int j=1;j<=n;j++){
            // int sa=strlen(a[j]);
            // int sb=strlen(b[i]);
            //坑点，如果传入的值是 a[j]+1,b 那么在check函数里面，字符的索引就得从0开始了，而不是1.巨坑！！！！
            bool f=check(a[j],b,k);
            if(f) ans++;
        }
        cout<<ans<<endl;
    }
    
    return 0;
}
```

### 区间DP

#### AcWing 282. 石子合并

挺难的，会写代码，但是还不太懂

枚举长度

```c++
#include<iostream>
#include<cstring>
using namespace std;

const int N=310;
int s[N];
int f[N][N];
int n;
int main(){
    cin>>n;
    memset(f,0x3f,sizeof f);
    for(int i=1;i<=n;i++) cin>>s[i];
    for(int i=1;i<=n;i++) s[i]+=s[i-1];
    
    for(int i=1;i<=n;i++) f[i][i]=0;
    
    //枚举长度
    
    for(int len=2;len<=n;len++){
        for(int i=1;i+len-1<=n;i++){
            int l=i,r=i+len-1;
            for(int k=l;k<r;k++){
                f[l][r]=min(f[l][r],f[l][k]+f[k+1][r]+s[r]-s[l-1]);
            }
        }
    }
    cout<<f[1][n]<<endl;
    return 0;
}
```

### 计数类DP

#### AcWing 900. 整数划分

用完全背包的思想去做

动态规划

1. 状态表示

   1. 集合：$f[i][j]$表示1-i所有数字组成j的方案
   2. 值：方案的个数

2. 状态转移，我们考虑数字i选取的个数

   1. 当i选0个的时候 $f[i][j]=f[i-1][j]$
   2. 当i选1个的时候 $f[i][j]=f[i-1][j-i]$
   3. 当i选2个的时候 $f[i][j]=f[i-1][j-2i]$
   4. ...
   5. 当i选s个的时候 $f[i][j]=f[i-1][j-s*i]$

   所以状态转移方程写成
   $$
   f[i][j]=f[i-1][j]+f[i-1][j-i]+f[i-1][j-2*i]+...+f[i-1][j-s*i]
   $$
   我们可以发现
   $$
   f[i][j-i]=f[i-1][j-i]+f[i-1][j-2*i]+...+f[i-1][j-s*i]
   $$
   把公式2带入公式1，可以写成
   $$
   f[i][j]=f[i-1][j]+f[i][j-i]
   $$
   然后我们进行降维操作，去掉一维
   $$
   f[j]=f[j]+f[j-i]
   $$
   为了保证每次更新的时候用的都是上一个状态，所以j要从小到大枚举，证明过程

   

```c++
#include<iostream>
using namespace std;
const int N=1010,mod=1e9+7;
int f[N];
int n;
int main(){
    cin>>n;
    f[0]=1;
    for(int i=1;i<=n;i++){
        for(int j=i;j<=n;j++){
            f[j]=(f[j]+f[j-i])%mod;
        }
    }
    cout<<f[n]<<endl;
    return 0;
}
```

### 状态压缩DP

#### AcWing 291. 蒙德里安的梦想

状态压缩入门题

这道题的思考方式，当我们把所有横着放的方案确定好之后，剩下的竖着方法方案一定是确定的，只要找空插竖着的方格即可，当然这要考虑合法的问题，所以，这个问题就转换成枚举所有横着摆放方格的方案了。

$f[i][j]$中，i代表第i列，j代表状态，一个用二进制表示的十进制数，二进制的位数取决于总共的行数，如果当前行有从前一列伸出来的方格，那么当前行置为1，不然就置为0。所以$f[i][j]$代表的集合是：有哪些行从i-1列伸到i列，并且使第i列的状态为j的方案数。

我们假设第i-1列的状态为:1001

1. 假设目标状态j的状态为1000，这个时候j&k!=0,有冲突，i-1列的第一行已经伸到i列了，所以第i列不可能伸出一行到i+1列。所以状态转移的第一个要求就是$j\&k==0$，即相邻两列不会有冲突，同一行只能伸出一次。
2. 假设目标状态j的状态0100，这个时候j&k==0，满足第一个要求，但是j|k=1101，剩下的空间（连续0的个数）不能摆放竖着的方格，是一个非法状态。所以第二个要求就是状态j|k中连续的0必须是偶数。
3. 假设j=0000，满足j&k==0,j|k=1001,合法，$f[i][j]+=f[i-1][k]$，当前状态j可以由i-1列的状态k转移过来。

看代码



```c++
#include<iostream>
#include<cstring>
#include<algorithm>
using namespace std;

const int N=12,M=1<<N;
bool st[M];
long long f[N][M];

int n,m;

int main(){
    while(cin>>n>>m,n||m){
        memset(f,0,sizeof f);
        //提前算出所有不合法的状态
        for(int i=0;i< 1<<n;i++){
            int cnt=0;
            st[i]=true;
            for(int j=0;j<n;j++){
                if(i>>j&1){
                    if(cnt&1){
                        st[i]=false;
                        break;
                    } 
                    cnt=0;
                }else cnt++;
                
            }
            if(cnt&1) st[i]=false;
        }
        f[0][0]=1;
        for(int i=1;i<= m;i++){
            for(int j=0;j<1<<n;j++){
                for(int k=0;k< 1<<n;k++){
                    //第一个条件，妹有冲突
                    //第二个条件，合法
                    if((j&k)==0 && st[j|k]){
                        f[i][j]+=f[i-1][k];
                    }
                }
            }
        }
        cout<<f[m][0]<<endl;
    }
    
    return 0;
}
```

预处理状态转移的条件，加速

```c++
#include<iostream>
#include<cstring>
#include<algorithm>
#include<vector>
using namespace std;

const int N=12,M=1<<N;
bool st[M];
long long f[N][M];
vector<int> state[M];
int n,m;

int main(){
    while(cin>>n>>m,n||m){
        memset(f,0,sizeof f);
        
        //提前算出所有不合法的状态
        for(int i=0;i< 1<<n;i++){
            int cnt=0;
            st[i]=true;
            for(int j=0;j<n;j++){
                if(i>>j&1){
                    if(cnt&1){
                        st[i]=false;
                        break;
                    } 
                    cnt=0;
                }else cnt++;
                
            }
            if(cnt&1) st[i]=false;
        }
        //预处理判断条件  加速效果明显 904ms->194ms
        for(int j=0;j< 1<<n;j++){
            state[j].clear();
            for(int k=0;k< 1<<n;k++){
                if((j&k)==0&&st[j|k]) state[j].push_back(k);
            }
        }
        f[0][0]=1;
        for(int i=1;i<= m;i++){
            for(int j=0;j<1<<n;j++){
                for(int k=0;k<state[j].size();k++){
                    f[i][j]+=f[i-1][state[j][k]];
                }
                // for(int k=0;k< 1<<n;k++){
                //     //第一个条件，妹有冲突
                //     //第二个条件，合法
                //     if((j&k)==0 && st[j|k]){
                //         f[i][j]+=f[i-1][k];
                //     }
                // }
            }
        }
        cout<<f[m][0]<<endl;
    }
    
    return 0;
}
```

#### AcWing 91. 最短Hamilton路径

暴力做法复杂度是 20!



```c++
#include<iostream>
#include<cstring>
#include<algorithm>
using namespace std;
const int N=21,M=1<<N;
int f[M][N],g[N][N];
int n;
int main(){
    cin>>n;
    for(int i=0;i<n;i++){
        for(int j=0;j<n;j++){
            cin>>g[i][j];
        }
    }
    memset(f,0x3f,sizeof f);
    f[1][0]=0;
    for(int i=0;i< 1<<n;i++){
        for(int j=0;j<n;j++){
            if((i>>j)&1){
                for(int k=0;k<n;k++){
                    if((i-(1<<j))>>k&1){
                        f[i][j]=min(f[i][j],f[i-(1<<j)][k]+g[k][j]);
                    }
                }
            }
        }
    }
    cout<<f[(1<<n)-1][n-1]<<endl;
    return 0;
}
```

### 树形DP

#### AcWing 285. 没有上司的舞会

这种题思维难度不大

比起前面几道DP题，这道题不难

```c++
#include<iostream>
#include<cstring>
using namespace std;

const int N=6010;
int happy[N];
int f[N][2];
int h[N],e[N],ne[N],idx;
bool has_father[N];
int n;

void add(int a,int b){
    e[idx]=b;
    ne[idx]=h[a];
    h[a]=idx++;
}

void dfs(int u){
    f[u][1]=happy[u];
    for(int i=h[u];i!=-1;i=ne[i]){
        int j=e[i];
        dfs(j);
        f[u][0]+=max(f[j][0],f[j][1]);
        f[u][1]+=f[j][0];
    }
}

int main(){
    cin>>n;
    for(int i=1;i<=n;i++){
        cin>>happy[i];
    }
    //邻接矩阵的头全部置为-1
    memset(h,-1,sizeof h);
    
    for(int i=0;i<n-1;i++){
        int a,b;
        cin>>a>>b;
        has_father[a]=1;
        add(b,a);
    }
    //找一下根节点，没有父节点的节点就是根节点
    int root=1;
    while(has_father[root]) root++;
    
    dfs(root);
    cout<<max(f[root][0],f[root][1])<<endl;
    
    return 0;
}

```

### 记忆化DP

#### AcWing 901. 滑雪

这道题更像是一道记忆化搜索

```c++
#include<cstring>
#include<iostream>
using namespace std;
const int N=310;
int n,m;
int h[N][N];
int f[N][N];
int res;
int d[4][2]={{0,1},{0,-1},{-1,0},{1,0}};
int dp(int x,int y){
    //这种写法，v就等价于f[x][y]，更新v就等价于更新f[i][j]
    int &v=f[x][y];
    if(v!=-1) return v;
    //当前路径没有走过，初始化为1
    v=1;
    for(int i=0;i<4;i++){
        int nx=x+d[i][0];
        int ny=y+d[i][1];
        if(nx>=1&&nx<=n&&ny>=1&&ny<=m&&h[nx][ny]<h[x][y]){
            v=max(v,dp(nx,ny)+1);
        }
    }
    return v;
    
    
}
int main(){
    scanf("%d%d",&n,&m);
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            scanf("%d",&h[i][j]);
        }
    }
    memset(f,-1,sizeof(f));
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            res=max(res,dp(i,j));
        }
    }
    printf("%d\n",res);
    return 0;
}
```

## 第六讲 贪心

### 区间问题

#### AcWing 905. 区间选点

策略，按照右端点排序

```c++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

const int N=1e5+10;
int n;

struct node{
    int l,r;
    bool operator < (const node& rhs )const{
        return r<rhs.r;
    }
};
vector<node> v;

int main(){
    scanf("%d",&n);
    for(int i=0;i<n;i++){
        node t;
        int a,b;
        scanf("%d%d",&a,&b);
        t.l=a;
        t.r=b;
        v.push_back(t);
    }
    sort(v.begin(),v.end());
    int cur=v[0].r;
    int ans=1;
    for(int i=0;i<n;i++){
        if(v[i].l<=cur){
            continue;
        }else{
            cur=v[i].r;
            ans++;
        }
    }
    printf("%d\n",ans);
    return 0;
}
```

#### AcWing 908. 最大不相交区间的数量

右端点排序，数数。

居然和上一道题做法一样，但是证明需要花点时间，但是就是能AC……

```c++
#include<iostream>
#include<algorithm>
using namespace std;
const int N=1e5+10;

int n;
struct Range{
    int l,r;
    bool operator <(const Range& rhs)const {
        return r<rhs.r;
    }
}range[N];

int main(){
    scanf("%d",&n);
    for(int i=0;i<n;i++){
        int a,b;
        scanf("%d%d",&a,&b);
        range[i]={a,b};
    }
    sort(range,range+n);
    int ans=0,ed=-2e9;
    for(int i=0;i<n;i++){
        if(ed<range[i].l){
            ed=range[i].r;
            ans++;
        }
    }
    printf("%d\n",ans);
    return 0;
    
}
```



### 模板

#### AcWing xxx. xxxx

```c++

```

