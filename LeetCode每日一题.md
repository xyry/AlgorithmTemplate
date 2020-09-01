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

