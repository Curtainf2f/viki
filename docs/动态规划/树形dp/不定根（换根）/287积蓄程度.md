# 287积蓄程度

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
 
const int N = 2e5+5;
const int INF = 0x7fffffff;
const LL MOD = 1e9+7;

int h[N], dp[N], d[N];
vector<pair<int, int> > edge[N];

// int dfs1(int x, int pre = -1, int flow = INF){
//     int f;
//     if(edge[x].size() == 1) f = INF;
//     else f = 0;
//     for(int i = 0; i < edge[x].size(); i ++){
//         int y = edge[x][i].first, c = edge[x][i].second;
//         if(y == pre) continue;
//         f += dfs1(y, x, min(c, flow));
//     }
//     if(f == INF) d[x] = 0;
//     else d[x] = f;
//     return min(f, flow);
// }

void dfs1(int x, int pre = -1){
    bool haveson = false;
    for(int i = 0; i < edge[x].size(); i ++){
        if(edge[x][i].first == pre) continue;
        haveson = true;
        dfs1(edge[x][i].first, x);
    }
    if(!haveson) d[x] = INF;
    else{
        for(int i = 0; i < edge[x].size(); i ++){
            if(edge[x][i].first == pre) continue;
            d[x] += min(edge[x][i].second, d[edge[x][i].first]);
        }
    }
}

void dfs2(int x, int pre = -1){
    for(int i = 0; i < edge[x].size(); i ++){
        int y = edge[x][i].first, c = edge[x][i].second;
        if(y == pre) continue;
        if(d[y] != INF)dp[y] = d[y] + min(dp[x] - min(d[y], c), c);
        else dp[y] = min(dp[x] - c, c);
        dfs2(y, x);
    }
}

int main(int argc, char const *argv[]) {
    int t;
    scanf("%d", &t);
    while(t --){
        int n;
        scanf("%d", &n);
        for(int i = 1; i < n; i ++){
            int a, b, c;
            scanf("%d%d%d", &a, &b, &c);
            edge[a].push_back({b, c});
            edge[b].push_back({a, c});
        }
        dfs1(1);
        dp[1] = d[1];
        dfs2(1);
        int ans = 0;
        for(int i = 1; i <= n; i ++){
            test(dp[i]);
            ans = max(ans, dp[i]);
        }
        printf("%d\n", ans);
        for(int i = 1; i <= n; i ++) edge[i].clear();
        memset(dp, 0, sizeof(dp));
        memset(d, 0, sizeof(d));
    }
    return 0;
}
```