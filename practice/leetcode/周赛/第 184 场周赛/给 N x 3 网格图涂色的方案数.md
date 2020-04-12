# 给 N x 3 网格图涂色的方案数

你有一个 n x 3 的网格图 grid ，你需要用 红，黄，绿 三种颜色之一给每一个格子上色，且确保相邻格子颜色不同（也就是有相同水平边或者垂直边的格子颜色不同）。

给你网格图的行数 n 。

请你返回给 grid 涂色的方案数。由于答案可能会非常大，请你返回答案对 10^9 + 7 取余的结果。

示例 1：
```
输入：n = 1
输出：12
```

示例 2：
```
输入：n = 2
输出：54
```
示例 3：
```
输入：n = 3
输出：246
```
示例 4：
```
输入：n = 7
输出：106494
```
示例 5：
```
输入：n = 5000
输出：30228214
```

提示：

n == grid.length
grid[i].length == 3
1 <= n <= 5000
```
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/number-of-ways-to-paint-n-x-3-grid
```

# 代码

```cpp
class Solution {
public:
    int dp[5555][22], MOD = 1e9+7;
    string s[22] = {"", "ryr", "ryg", "rgr", "rgy", "yry", "yrg", "ygr", "ygy", "gry", "grg", "gyr", "gyg"};
    int numOfWays(int n) {
        for(int i = 1; i <= 12; i ++) dp[1][i] = 1;
        for(int i = 2; i <= n; i ++){
            for(int j = 1; j <= 12; j ++){
                dp[i][j] = 0;
                for(int k = 1; k <= 12; k ++){
                    string s1 = s[j], s2 = s[k];
                    bool ok = true;
                    for(int i = 0; i < 3; i ++){
                        if(s1[i] == s2[i]){
                            ok = false;
                            break;
                        }
                    }
                    if(ok){
                        dp[i][j] += dp[i-1][k];
                        dp[i][j] %= MOD;
                    }
                }
            }
        }
        int ans = 0;
        for(int i = 1; i <= 12; i ++){
            ans += dp[n][i];
            ans %= MOD;
        }
        return ans;
    }
};
```

# 思路

种类数dp

+ 根据题目给的方案，只考虑一层，将一层中的所有可能枚举出来，用一个数字代表一种情况
例如代码中s数组，r代表红色，y代表黄色，g代表绿色。下标从1~12代表全部十二种情况

+ 二层及二层以上的，只要这层的其中一种方案每个位置的颜色都与上一层其中一种方案的每个位置颜色分别不同，那么直接把上一层方案的情况数，加到这一层方案的情况数上即可。

+ 最后将12种可能结尾的情况全部加起来就是答案

复杂度$O(n)$ 常数 432