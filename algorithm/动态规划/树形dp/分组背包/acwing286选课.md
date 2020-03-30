# 286选课

```cpp
#include <bits/stdc++.h>
 
using namespace std;
typedef long long LL;
typedef long double LD;
typedef unsigned long long ULL;
 
#ifdef _LOCAL
#define test(x) cerr << "[" << #x << ": " << x << "] (" << __LINE__ << ", in `" << __FUNCTION__ << "`)\n"
struct Tester {
    Tester() { freopen("data.in", "r", stdin); }
    ~Tester() { fprintf(stderr, "\nTime: %d ms\n", int(1000.0 * clock() / CLOCKS_PER_SEC)); }
} _shower;
#else
#define test(x)
#endif
 
const int N = 333;
const LL MOD = 1e9+7;

int n, m;
int score[N], fa[N], dp[N][N];
vector<int> son[N];

void dfs(int x){
    for(int i = 0; i <= m; i ++){
        dp[x][i] = 0;
    }
    for(int i = 0; i < son[x].size(); i ++){
        int y = son[x][i];
        dfs(y);
        for(int t = m; t >= 0; t --){ // 要用多大的背包
            for(int j = t; j >= 0; j --){ // 塞多大容量的y
                if(t - j >= 0) dp[x][t] = max(dp[x][t], dp[x][t-j] + dp[y][j]);
            }
        }
    }
    if(x != 0){
        for(int t = m; t >= 1; t --){
            dp[x][t] = dp[x][t-1] + score[x];
        }
    }
}

int main(int argc, char const *argv[]) {
    scanf("%d%d", &n, &m);
    for(int i = 1; i <= n; i ++){
        int a;
        scanf("%d%d", &a, &score[i]);
        fa[i] = a;
        son[a].push_back(i);
    }
    dfs(0);
    int ans = 0;
    for(int i = 0; i <= m; i ++){
        ans = max(dp[0][i], ans);
    }
    printf("%d\n", ans);
    return 0;
}
```