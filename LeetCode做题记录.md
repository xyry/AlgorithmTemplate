[TOC]

用来记录除去每日一题之外的题目。

可以用来补周赛的题目。

现在周赛的目标是保二追三，保证做出两道题，尽量做出三道题，第四道题战略性放弃。

### 第207场周赛

您的最新评分为1681，上次为1694，比赛只做出了一道题

| 排名        | 用户名 | 得分 | 完成时间 | [题目1 (3)](https://leetcode-cn.com/contest/weekly-contest-207/problems/rearrange-spaces-between-words/) | [题目2 (4)](https://leetcode-cn.com/contest/weekly-contest-207/problems/split-a-string-into-the-max-number-of-unique-substrings/) | [题目3 (5)](https://leetcode-cn.com/contest/weekly-contest-207/problems/maximum-non-negative-product-in-a-matrix/) | [题目4 (6)](https://leetcode-cn.com/contest/weekly-contest-207/problems/minimum-cost-to-connect-two-groups-of-points/) |
| ----------- | ------ | ---- | -------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1529 / 4115 | xyry   | 3    | 0:20:17  | 0:15:17 1                                                    |                                                              |                                                              |                                                              |

#### [1592. 重新排列单词间的空格](https://leetcode-cn.com/problems/rearrange-spaces-between-words/) （3）√

签到题，写了蛮久

```c++
class Solution {
public:
    string reorderSpaces(string text) {
        string ans="";
        int cnt=0;
        int n=text.size();
        //vs里面存放每一个单词
        vector<string> vs;
        string tmp="";
        for(int i=0;i<n;i++){
            if(text[i]!=' '){
                tmp+=text[i];
            }else{
                //cnt统计空格数量
                cnt++;
                if(tmp!="") vs.push_back(tmp);
                tmp="";
            }
        }
        if(tmp!="") vs.push_back(tmp);
        // for(int i=0;i<vs.size();i++) cout<<vs[i]<<endl;
        //平均空格数量
        int ave;
        //如果只有一个单词，所有空格都放在该单词后面
        if(vs.size()==1) ave=cnt;
        //否则正常计算
        else ave=cnt/(vs.size()-1);
        
        for(int i=0;i<vs.size();i++){
            
            ans+=vs[i];
            //如果单词数量大于1并且该单词是最后一个单词，就要退出循环，
            //把剩下的空格全部加到最后
            if(i&&i==vs.size()-1) break;
            //tcnt记录现在放了多少个空格，一共要放ave个空格
            int tcnt=0;
            while(tcnt<ave){
                tcnt++;
                ans+=' ';
            }
        }

        int tcnt=0;
        //如果单词数量大于1,如果只有一个单词，上面的处理就已经把所有空格都加到单词后面了，这里处理的是除不尽的情况
        if(vs.size()!=1)
            //取模就是除不尽剩下的空格
            while(tcnt<(cnt%(vs.size()-1))){
                tcnt++;
                ans+=' ';
            }
        return ans;
    }
};
```



#### [1593. 拆分字符串使唯一子字符串的数目最大](https://leetcode-cn.com/problems/split-a-string-into-the-max-number-of-unique-substrings/)  （4）√

看到字符串长度才16，一定要想到用暴力做，

思路就是dfs+回溯。

枚举每个长度，利用哈希判断每个字符串是否出现。

```c++
class Solution {
public:
    int ans=-1;
    int n;
    unordered_set<string> us;
    void dfs(string s,int cnt){
        if(s==""){
            ans=max(ans,cnt);
            return;
        }
        for(int i=1;i<=s.size();i++){
            string tmp=s.substr(0,i);
            if(us.count(tmp)) continue;
            else{
                us.insert(tmp);
                dfs(s.substr(i),cnt+1);
                us.erase(tmp);
            }
        }
        return;
    }

    int maxUniqueSplit(string s) {
        dfs(s,0);
        return ans;
    }
};
```



#### [1594. 矩阵的最大非负积](https://leetcode-cn.com/problems/maximum-non-negative-product-in-a-matrix/) （5）√

又是一道看似搜索，实则动态规划的题目。

做的时候，BFS一直超时，我就应该明白了。

这种存在负数的，每个状态就要记录一个最大值和一个最小值，这种题做过3遍了，比赛的时候都没想出来。

```c++
class Solution {
public:
    const int mod =1e9+7;
    long long dp[20][20][2];
    int maxProductPath(vector<vector<int>>& grid) {
        int n=grid.size();
        int m=grid[0].size();
        //起点和终点有一个为0，结果就为0
        if(grid[0][0]==0||grid[n-1][m-1]==0) return 0;
        //第0维代表最小值 第1维代表最大值
        dp[0][0][0]=dp[0][0][1]=grid[0][0];
        //初始化最左边一列
        for(int i=1;i<n;i++){
            int cur=grid[i][0];
            if(cur==0){
                dp[i][0][0]=dp[i][0][1]=0;
            }else if(cur<0){
                dp[i][0][0]=dp[i-1][0][1]*cur;
                dp[i][0][1]=dp[i-1][0][0]*cur;
            }else if(cur>0){
                dp[i][0][0]=dp[i-1][0][0]*cur;
                dp[i][0][1]=dp[i-1][0][1]*cur;
            }
        }
        //初始化最上面一行
        for(int j=1;j<m;j++){
            int cur=grid[0][j];
            if(grid[0][j]==0){
                dp[0][j][0]=dp[0][j][1]=0;
            }else if(grid[0][j]<0){
                dp[0][j][0]=dp[0][j-1][1]*cur;
                dp[0][j][1]=dp[0][j-1][0]*cur;
            }else if(grid[0][j]>0){
                dp[0][j][0]=dp[0][j-1][0]*cur;
                dp[0][j][1]=dp[0][j-1][1]*cur;
            }
        }
        //每个格子只能由上面或者左边转移过来
        //dp[i][j],dp[i-1][j]或者dp[i][j-1]
        for(int i=1;i<n;i++){
            for(int j=1;j<m;j++){
                int cur=grid[i][j];
                if(cur==0){
                    dp[i][j][0]=dp[i][j][1]=0;
                }else if(cur<0){
                    dp[i][j][0]=max(dp[i-1][j][1],dp[i][j-1][1])*cur;
                    dp[i][j][1]=min(dp[i-1][j][0],dp[i][j-1][0])*cur;
                }else if(cur>0){
                    dp[i][j][0]=min(dp[i-1][j][0],dp[i][j-1][0])*cur;
                    dp[i][j][1]=max(dp[i-1][j][1],dp[i][j-1][1])*cur;
                }
            }
        }
        if(dp[n-1][m-1][1]<0) return -1;
        return dp[n-1][m-1][1]%mod;

    }
};
```

