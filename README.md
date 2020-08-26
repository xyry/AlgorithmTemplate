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

## 整数二分

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

## 浮点二分

2020年8月9日

```c++
bool check(double x) {/* ... */} // 检查x是否满足某种性质

double bsearch_3(double l, double r)
{
    const double eps = 1e-6;   // eps 表示精度，取决于题目对精度的要求
    while (r - l > eps)
    {
        double mid = (l + r) / 2;
        if (check(mid)) r = mid;
        else l = mid;
    }
    return l;
}
```

## 高精度

2020年8月9日

### 高精度加法

A,B中数是倒着存的，也就是A[0]是个位，A[1]是十位，依次类推。考虑进位就可以了

```c++

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
```

### 高精度减法

减法存在一个大的减小的问题，所以在做减法之前，要把数据处理成大的数字在前面

if(A>=B)   sub(A,B)

else sub(B,A)//输出的时候最前面加一个负号即可

减的时候需要注意借位

```c++
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
    //去掉前导零，不要忘记了
    while(C.size()>1&&C.back()==0) C.pop_back();
    return C;
}
```



### 高精度乘法

大数乘以一个小一点的数。

注意最后一个进位，如果A处理完了，可能还有进位，要处理。注意去掉前导零。

```c++
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
```



### 高精度除法

与前面三个模板有区别，C是正着存数据的，所以最后要把C反过来，然后去除前导零。

```c++
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
```

## 前缀和与差分

### 一维前缀和 —— 模板题 AcWing 795. 前缀和

S[i] = a[1] + a[2] + ... a[i]
a[l] + ... + a[r] = S[r] - S[l - 1]

### 二维前缀和 —— 模板题 AcWing 796. 子矩阵的和

S[i, j] = 第i行j列格子左上部分所有元素的和
以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵的和为：
S[x2, y2] - S[x1 - 1, y2] - S[x2, y1 - 1] + S[x1 - 1, y1 - 1]
### 一维差分 —— 模板题 AcWing 797. 差分
给区间[l, r]中的每个数加上c：B[l] += c, B[r + 1] -= c
### 二维差分 —— 模板题 AcWing 798. 差分矩阵
给以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵中的所有元素加上c：
S[x1, y1] += c, S[x2 + 1, y1] -= c, S[x1, y2 + 1] -= c, S[x2 + 1, y2 + 1] += c

## 双指针算法

核心思想把朴素的$O(n^2)$算法优化到$O(n)$。

典型的例子，给一个英文句子，开头是字母，每个单词之间用空格分开，要求输出每一个单词，每个单词占一行。

这种情况就可以使用双指针。

## 位运算

```c++
求n的第k位数字: n >> k & 1
返回n的最后一位1：lowbit(n) = n & -n
```

## 单链表

数组模拟单链表，算法中竞赛大多数都是这么做。

快，因为数组模拟单链表相当于静态链表，用指针实现的话，new的操作开辟动态空间很慢。

## 并查集

```c++
p[N]记得初始化
for(int i=1;i<=n;i++) p[i]=i;

//查找父节点+路径压缩
int find(int x){
    if(p[x]!=x) p[x]=find(p[x]);
    return p[x];
}
```

## 堆排序

```c++
核心操作就是down操作和up操作

//建堆操作从n/2开始
for(int i=n/2;i>=1;i--) down(i);
    
//删除操作
swap(h[1],h[size]);
down(1);


void down(int u){
    int t=u;
    if(u*2<=s&&h[u*2]<h[t]) t=u*2;
    if(u*2+1<=s&&h[u*2+1]<h[t]) t=u*2+1;
    if(t!=u){
        swap(h[u],h[t]);
        down(t);
    }
}

void up(int u){
    int t=u;
    if(u/2&&h[t]<h[u/2]) t=u/2;
    if(t!=u){
        swap(h[t],h[u]);
        up(t);
    }
}

```



## 哈希表

### 拉链法

```c++
const int N=10003;
//该值设置为一个质数，并且离2的n次幂越远越好，这样冲突概率最小。
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
    //从该链表开始找这个数
    for(int i=h[k];i!=-1;i=ne[i]){
        if(e[i]==x) return true;
    }
    return false;
}
```

### 开放寻址法

```c++
const int N=200003,null=0x3f3f3f3f;
//用null表示h[]中没有存放数，也就是初始值
int h[N];
//开放寻址法

//找到在哪里放x 
//如果x已经存在于h[]数组中，那么返回x放的位置k,
//如果x不存在于h[]数组中，返回x要放的位置k
int find(int x){
    int k=(x%N+N)%N;
    while(h[k]!=null&&h[k]!=x){
        k++;
        //如果k到了数组最后一个位置，从头开始重新找
        if(k==N) k=0;
    }
    return k;    
}
```

### 字符串哈希

字符串前缀哈希，用来解决字符串的难题。

要求字符对应的值不能为0，不然会有冲突。

P一般取131或者13331

Q一般取2的64次方，在代码里用ULL表示，这样溢出就相当于取模了。

非常巧的做法

```c++
#include<iostream>
typedef unsigned long long ULL;
using namespace std;

const int N=1e5+10,P=131;
int n,m;
char str[N];
ULL h[N],p[N];

ULL get(int l,int r){
    //获取字符串str[l:r]范围的哈希值
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

## Dijkstra

单源最短路径

### 朴素n^2模板

```c++
//稠密图
//标记
bool st[N];
//距离
int dist[N];
//邻接矩阵
int g[N][N];
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
```

### 堆优化 mlogn

```c++
//堆优化版本，没事看看
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
```

## Bellman-Ford

如果存在负权环，最短路不一定存在。也就是说一个环的长度加起来是个负数，这样从起点可能走不到终点。

要经常看下这个算法，松弛操作。

```c++
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
```

## SPFA

### 标准模板

和堆优化dijkstra很像

```C++
//因为是稀疏图，这里用的邻接表存储
//st代表该点是否在队列中，如果在就不往里面添加。
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
```

### 判负环

```c++
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
```

## Floyd

多源最短路径

```c++
//初始化的时候
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
void floyd(){
    for(int k=1;k<=n;k++){
        for(int i=1;i<=n;i++){
            for(int j=1;j<=n;j++){
                d[i][j]=min(d[i][j],d[i][k]+d[k][j]);
            }
        }
    }
}

```

## Prim

```c++
//g数组初始化
memset(g,INF,sizeof g);
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
```

## Kruskal

求最小生成树算法，对所有边排序，每次加入一条最短的边，（prim算法是每次加入一个点），利用并查集来做，如果最后加入了n-1条边，那么这n个点是连通的。

## 二分图

二分图当且仅当图中不含奇数环，（有奇数条边的环，因为会推出矛盾）。

由于图中不含奇数环，所以染色过程中一定没有矛盾。

## 匈牙利算法

二分图的最大匹配，递归思想。

```c++
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
```

