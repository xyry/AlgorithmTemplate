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

