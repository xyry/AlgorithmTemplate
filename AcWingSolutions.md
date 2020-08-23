[TOC]

AcWing 做题记录

# 算法基础课

## 基础算法

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

## 数据结构

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



### 模板

#### AcWing xxx. xxxx

```c++

```

