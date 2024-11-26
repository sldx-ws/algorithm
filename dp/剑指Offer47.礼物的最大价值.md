## 剑指Offer47.礼物的最大价值

在⼀个`m*n`的棋盘的每⼀格都放有⼀个礼物，每个礼物都有⼀定的价值（价值⼤于`0`）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动⼀格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？



**示例 1：**

```
示例1:
输⼊:
[
 [1,3,1],
 [1,5,1],
 [4,2,1]
]
输出:12
解释:路径1→3→5→2→1可以拿到最多价值的礼物
```

## 解法

```cc
class Solution 
{
public:
	int maxValue(vector<vector<int>>& grid){
    	int m = grid.size();
     int n = grid[0].size();
    	vector<vector<int>> dp(m + 1, vector<int>(n + 1));
        
    	for (int i = 1; i < m + 1; ++i)
     {
				     for (int j = 1; j < n + 1; ++j)
         {
             dp[i][j] = grid[i-1][j-1] + max(dp[i][j-1], dp[i-1][j]);
         }
     }
        
    	return dp[m][n];
	}
};
```

