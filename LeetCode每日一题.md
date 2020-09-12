[TOC]



## 9月

### 9月1日  [486. 预测赢家](https://leetcode-cn.com/problems/predict-the-winner/) √

做法：dfs+记忆化

只有20个数字，所以可以暴力搜索。dfs的时候会有很多重复的情况，所以只要加一个记忆化搜索，时间就可以超过100%了。

代码思路是从这个题解中出来的，看了思路自己写的代码， https://leetcode-cn.com/problems/predict-the-winner/solution/ling-he-bo-yi-ji-yi-hua-0ms-gao-ding-by-time-limit/ 

AC代码

```c++
class Solution {
public:
//dfs+记忆化搜索，真的牛皮，速度立刻就上来了，好题
    int dp[25][25];
    int pre[25];
    int dfs(int l,int r,vector<int>& nums){
        if(dp[l][r]!=-1) return dp[l][r];
        if(l==r) return dp[l][r]=nums[l];
        else{
            // int sum=pre[r+1]-pre[l+1-1];
            // return sum-min(dfs(l+1,r,nums),dfs(l,r-1,nums));
            return dp[l][r]=pre[r+1]-pre[l+1-1]-min(dfs(l+1,r,nums),dfs(l,r-1,nums));
        }
    }
    bool PredictTheWinner(vector<int>& nums) {
        memset(dp,-1,sizeof(dp));
        int n=nums.size();
        for(int i=0;i<n;i++){
            // sum+=nums[i];
            //pre[1..n] 
            pre[i+1]=pre[i]+nums[i];
        }
        int a=dfs(0,n-1,nums);
        if(pre[n]-a>a) return false;
        else return true;
    }
};
```

### 9月2日  [剑指 Offer 20. 表示数值的字符串](https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/) √

python 直接做

```python
class Solution(object):
    def isNumber(self, s):
        """
        :type s: str
        :rtype: bool
        """
        try:
            float(s)
        except:
            return False;
        return True;
```

但这如果是一道面试题，这样做直接回家了😂

挨个判断字符，思路来自 https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/solution/czhu-zi-fu-jian-cha-by-snowd/ 

```c++
class Solution {
public:
    bool isNumber(string s) {
        bool hasNum=false,hasE=false,hasOp=false,hasDot=false;
        int index=-1;
        int n=s.size();
        //去掉前置空格
        while(index<n&&s[++index]==' ');
        while(index<n){
            if(s[index]>='0'&&s[index]<='9'){
                hasNum=1;
            }else if(s[index]=='E'||s[index]=='e'){
                if(hasE||!hasNum) return false;
                //E前后的数字单独判断，后面的数字不能为小数
                //所以出现E之后，其余三个状态恢复初始化
                hasE=1;
                hasNum=0;
                hasOp=0;
                hasDot=0;
            }else if(s[index]=='+'||s[index]=='-'){
                //正负号前面不能有数字，不能有点
                if(hasNum||hasDot) return false;
                hasOp=1;
            }else if(s[index]=='.'){
                //E/e后面不能有点
                //一个数字只能有一个点
                if(hasE||hasDot) return false;
                hasDot=1;
            }else if(s[index]==' '){
                //当前为空格直接退出
                break;
            }else{
                return false;
            }
            index++;
        }
        //去除后置空格
        while(index<n&&s[++index]==' ');
        // cout<<index<<endl;
        //必须有数字，排除只有一个点，一个Op。长度要为n。
        return hasNum&&index==n;
    }
};
```

### 9月3日  [51. N 皇后](https://leetcode-cn.com/problems/n-queens/) √

其实就是dfs，每行每列正对角线反对角线都要搜一遍，即可。记得回溯。

```c++
class Solution {
public:
    vector<string> g;
    vector<bool> row,col,dg,udg;
    vector<vector<string>> ans;
    int N;
    void dfs(int u){
        if(u==N){
            ans.push_back(g);
            return;
        }
        for(int i=0;i<N;i++){
            if(!col[i]&&!dg[u+i]&&!udg[N+u-i]){
                g[u][i]='Q';
                row[u]=1;
                col[i]=1;
                dg[u+i]=1;
                udg[N+u-i]=1;
                dfs(u+1);
                g[u][i]='.';
                row[u]=0;
                col[i]=0;
                dg[u+i]=0;
                udg[N+u-i]=0;
            }
        }
    }
    vector<vector<string>> solveNQueens(int n) {
        N=n;
        //初始化棋盘格
        for(int i=0;i<n;i++){
            string tmp="";
            for(int j=0;j<n;j++){
                tmp+='.';
            }
            g.push_back(tmp);
        }
        // cout<<g[0][0]<<endl;
        row.resize(2*n);
        col.resize(2*n);
        dg.resize(2*n);
        udg.resize(2*n);
        //按照row访问
        dfs(0);
        
        return ans;
    }
};
```

### 9月4日 [257. 二叉树的所有路径](https://leetcode-cn.com/problems/binary-tree-paths/) √

dfs+回溯 

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<string> ans;
    vector<int> v;

    void dfs(TreeNode* node){
        v.push_back(node->val);
        if(node->left==NULL&&node->right==NULL){
            
            string t="";
            for(int i=0;i<v.size();i++){
                if(i==0){
                    t+=to_string(v[i]);
                }else{
                    t+="->"+to_string(v[i]);
                }
            }
            ans.push_back(t);
            v.pop_back();
            return;
        }else{
            if(node->left!=NULL){
                dfs(node->left);
            }
            if(node->right!=NULL){
                dfs(node->right);
            }

        }
        //回溯
        v.pop_back();
        return;
    }

    vector<string> binaryTreePaths(TreeNode* root) {
        if(root==NULL) return ans;
        dfs(root);        
        return ans;
    }
};
```



### 9月5日 [60. 第k个排列](https://leetcode-cn.com/problems/permutation-sequence/) √

不能暴力dfs，会超时。

使用数学方法去做。

```c++
class Solution {
public:
    //预处理阶乘结果
    int f[15];
    int vis[15];
    int N,K;
    string ans="";
    int sum;
    void dfs(int u,int c,bool& flag){
        if(flag) return;
        vis[u]=1;
        // cout<<u<<" "<<c<<" "<<flag<<endl;
        ans+=to_string(u);
        if(c==N){
            flag=1;
            return;
        }
        for(int i=1;i<=N;i++){
            if(!vis[i]){
                // int cnt=N-c;
                // cout<<N-c<<endl;
                // cout<<f[cnt]<<endl;
                
                if(sum+f[N-c-1]<K){
                    sum=sum+f[N-c-1];
                    continue;
                }
                else{
                    dfs(i,c+1,flag);
                }
            }
            // if(flag) return;
        }
        return;
    }

    string getPermutation(int n, int k) {
        f[1]=1;
        f[0]=1;
        N=n;
        K=k;
        bool flag=false;
        sum=0;
        memset(vis,0,sizeof vis);
        for(int i=2;i<=n;i++) f[i]=i*f[i-1];
        // for(int i=2;i<=n;i++) cout<<"f["<<i<<"]="<<f[i]<<endl;
        
        for(int i=1;i<=n;i++){
            int cnt=n-1;
            
            if(sum+f[cnt]<k){
                sum+=f[cnt];
                continue;
            } 
            else{
                dfs(i,1,flag);
            } 
        }
        return ans;
        
    }
};
/*

//暴力会T
    int vis[15];
    int N;
    vector<string> v;
    void dfs(int u,int c,string t){
        vis[u]=1;
        t+=to_string(u);
        if(c==N){
            v.push_back(t);
            return;
        }
        for(int i=1;i<=N;i++){
            if(!vis[i]){
                dfs(i,c+1,t);
                vis[i]=0;
            }
        }
        vis[u]=0;
    }
  */
```

### 9月6号 [107. 二叉树的层次遍历 II](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/) √

BFS，技巧在于，和一般的bfs不一样，每次处理一层的节点，而不是一个一个处理。

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int>> ans;
        if(root==NULL) return ans;
        queue<TreeNode*> q;

        q.push(root);
        while(!q.empty()){
            vector<int> level;
            int size=q.size();
            for(int i=0;i<size;i++){
                TreeNode* cur=q.front();
                level.push_back(cur->val);
                q.pop();
                if(cur->left){
                    q.push(cur->left);
                }
                if(cur->right){
                    q.push(cur->right);
                    
                }
            }
            ans.push_back(level);
        }
        reverse(ans.begin(),ans.end());
        return ans;
    }
};
```



### 9月7日 [347. 前 K 个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/) √

思路：维护一个大小为k的小根堆，当元素数量=k的时候 ，判定，如果新进来的元素 出现次数大于堆顶元素出现的个数，那么丢掉堆顶，加入新的元素。学习了有限队列的写法。

```c++
class Solution {
public:

    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int,int> mp;
        for(int i=0;i<nums.size();i++){
            mp[nums[i]]++;
        }
        priority_queue<pair<int,int>,vector<pair<int,int>>,greater< pair<int,int> > > pq;
        for(auto iter=mp.begin();iter!=mp.end();iter++){
            if(pq.size()==k){
                if(iter->second > pq.top().first){
                    pq.pop();
                    pq.push(make_pair(iter->second,iter->first));
                }
            }else{
                pq.push(make_pair(iter->second,iter->first));
            }
            
        }
        vector<int> ans;
        while(!pq.empty()){
            ans.push_back(pq.top().second);
            pq.pop();
        }
        return ans;
    }
};
```



### 9月8日 [77. 组合](https://leetcode-cn.com/problems/combinations/) √

dfs+回溯+剪枝

剪枝的操作是以长度k来操作，如果剩下的数字个数不足以一个k长度的组合，break.

```c++
class Solution {
public:
    vector<vector<int>> ans;
    vector<bool> vis;
    int K,N;
    void dfs(int c,int u,vector<int> v){
        
        if(u==K){
            ans.emplace_back(v);
            return;
        }

        for(int i=c;i<=N;i++){
            if(!vis[i]){
                vis[i]=1;
                v.emplace_back(i);
                dfs(i,u+1,v);
                vis[i]=0;
                v.pop_back();
            }
        }
        
    }
    vector<vector<int>> combine(int n, int k) {
        if(n==0||k>n) return ans;
        if(k==0){
            ans.push_back({});
            return ans;
        }
        
        K=k;
        N=n;
        vis.resize(n+5,false);
        vector<int> v;

        for(int i=1;i<=n;i++){
            //剪枝操作
            if(n-i+1<k) break;
            v.emplace_back(i);
            vis[i]=1;
            dfs(i,1,v);
            vis[i]=0;
            v.pop_back();
        }
        return ans;
    }
};
```

### 9月9日 [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/) √

dfs+回溯 +一点点剪枝

题目数据太弱了，不过我也没有更好的处理比较复杂数据的办法

```c++
class Solution {
public:
//递归500层？可以实现？先写个试试
/*
我自己用这个数据测就T了， 提交居然过了？以后管他的，先提交一发试试算了，说不定数据贼弱呢……
[1,2,3,4]
500
*/ 
    vector<vector<int>> ans;
    vector<int> v;
    void dfs(int id,int sum,int target,vector<int> candidates){
        if(sum==target){
            ans.push_back(v);
            return;
        }
        for(int i=id;i<candidates.size();i++){
            if(target-sum>=candidates[i]){
                v.push_back(candidates[i]);
                dfs(i,sum+candidates[i],target,candidates);
                v.pop_back();
            }else{
                break;
            }
        }
        return;
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        sort(candidates.begin(),candidates.end());
        int size=candidates.size();
        for(int i=0;i<size;i++){
            if(candidates[i]<=target){
                v.push_back(candidates[i]);
                dfs(i,candidates[i],target,candidates);
                v.pop_back();
            }else{
                break;
            }
        }
        return ans;
    }
};
```

### 9月10日 [40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/) √

题目和昨天差不多，区别在于，数组中出现的每一个数字只能用一次，并且要考虑去重的问题。

用set去重即可。

```c++
class Solution {
public:
    int N;
    set<vector<int>> st;
    void dfs(int id,int target,vector<int>& v,vector<vector<int>>& ans,vector<int>& candidates){
        if(target==0){
            // ans.push_back(v);
            st.insert(v);
            return;
        }
        for(int i=id+1;i<N;i++){
            if(target>=candidates[i]){
                v.push_back(candidates[i]);
                dfs(i,target-candidates[i],v,ans,candidates);
                v.pop_back();
            }
        }
        return;
    }
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        
        sort(candidates.begin(),candidates.end());
        vector<int> v;
        vector<vector<int>> ans;
        N=candidates.size();
        for(int i=0;i<candidates.size();i++){
            v.push_back(candidates[i]);
            dfs(i,target-candidates[i],v,ans,candidates);
            v.pop_back();
        }  
        for(auto iter=st.begin();iter!=st.end();iter++){
            ans.push_back(*iter);
        }
        return ans;  
    }
};
```

### 9月11日 [216. 组合总和 III](https://leetcode-cn.com/problems/combination-sum-iii/) √

dfs+回溯

```c++
class Solution {
public:
    vector<vector<int>> ans;
    vector<int> v;
    int K;
    void dfs(int u,int id,int target){
        if(u==K){
            if(target==0) ans.push_back(v);
            return;
        }
        for(int i=id+1;i<=9;i++){
            if(id<=target){
                v.push_back(i);
                dfs(u+1,i,target-i);
                v.pop_back();
            }else{
                break;
            }
            
        }
        
    }
    vector<vector<int>> combinationSum3(int k, int n) {
        K=k;
        if(n<1||n>45){
            return ans;
        }
        for(int i=1;i<=9;i++){
            v.push_back(i);
            dfs(1,i,n-i);
            v.pop_back();
        }
        
        return ans;
    }
};
```

### 9月12日 [637. 二叉树的层平均值](https://leetcode-cn.com/problems/average-of-levels-in-binary-tree/) √

用BFS实现层次遍历

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<double> averageOfLevels(TreeNode* root) {
        vector<double> ans;
        if(!root) return ans;
        queue<TreeNode*> q;
        q.push(root);
        while(q.size()){
            long long sum=0;
            int size=q.size();
            for(int i=0;i<size;i++){
                TreeNode* t=q.front();
                q.pop();
                sum+=t->val;
                if(t->left) q.push(t->left);
                if(t->right) q.push(t->right);
            }
            ans.push_back(sum*1.0/size*1.0);
        }
        return ans;
    }
};
```

