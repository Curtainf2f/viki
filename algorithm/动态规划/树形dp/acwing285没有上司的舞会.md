# acwing285没有上司的舞会

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
 
const int N = 6005;
const LL MOD = 1e9+7;

int h[N], fa[N], dp[N][2];
vector<int> son[N];
bool vis[N];

void dfs(int x){
    dp[x][0] = 0;
    dp[x][1] = h[x];
    for(int i = 0; i < son[x].size(); i ++){
        int y = son[x][i];
        dfs(y);
        dp[x][0] += max(dp[y][1], dp[y][0]);
        dp[x][1] += dp[y][0];
    }
}

int main(int argc, char const *argv[]) {
    int n;
    scanf("%d", &n);
    for(int i = 1; i <= n ; i ++){
        scanf("%d", &h[i]);
    }
    int a, b;
    while(~scanf("%d%d", &a, &b) && a + b){
        fa[a] = b;
        vis[a] = true;
        son[b].push_back(a);
    }
    for(int i = 1; i <= n; i ++){
        if(!vis[i]){
            son[0].push_back(i);
        }
    }
    dfs(0);
    printf("%d\n", dp[0][0]);
    return 0;
}
```