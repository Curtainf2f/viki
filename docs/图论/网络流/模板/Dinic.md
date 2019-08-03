# Dinic
```cpp
const int N = 22;
const int M = 2222;
const int INF = 0x3f3f3f3f;

class Graphics{
private:
    struct Edge{
        int to, next, flow;
    }edge[M];
    int n;
    int first[N], sign;
    int depth[N], cur[N];
    int start, end;
    void addEdgeOneWay(int u, int v, int flow){
        edge[sign].next = first[u];
        edge[sign].flow = flow;
        edge[sign].to = v;
        first[u] = sign ++;
    }
    bool bfs(){
        memset(depth, 0, sizeof(int)*(n+1));
        queue<int> que;
        que.push(start); depth[start] = 1;
        while(!que.empty()){
            int now = que.front();
            que.pop();
            for(int i = first[now]; i != -1; i = edge[i].next){
                int to = edge[i].to, flow = edge[i].flow;
                if(!depth[to] && flow){
                    depth[to] = depth[now] + 1;
                    que.push(to);
                    if(to == end) return true;
                }
            }
        }
        return false;
    }
    int dfs(int now, int pFlow){
        if(now == end) return pFlow;
        for(int &i = cur[now]; i != -1; i = edge[i].next){
            int to = edge[i].to;
            if(depth[to] == depth[now] + 1 && edge[i].flow){
                int flow = dfs(to, min(edge[i].flow, pFlow));
                if(flow){
                    edge[i].flow -= flow;
                    edge[i^1].flow += flow;
                    return flow;
                }
            }
        }
        return 0;
    }
public:
    Graphics(int s, int e){clear(s, e);}
    Graphics(){clear(1, 1);}
    void setN(int n){
        this->n = n;
    }
    void clear(int s, int t){
        sign = 0;
        n = N - 1;
        start = s;
        end = t;
        memset(first, 0xff, sizeof(int)*(n+1));
    }
    void addEdgeFlowWay(int u, int v, int flow){
        addEdgeOneWay(u, v, flow);
        addEdgeOneWay(v, u, 0);
    }
    int dinic(){
        int maxFlow = 0;
        while(bfs()){
            memcpy(cur, first, sizeof(int)*(n+1));
            while(int flow = dfs(start, INF)){
                maxFlow += flow;
            }
        }
        return maxFlow;
    }
};
```