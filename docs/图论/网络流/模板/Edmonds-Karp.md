# Edmonds-Karp
```cpp
const int N = 22;
const int M = 2222;
const int INF = 0x3f3f3f3f;

class Graphics{
private:
    struct Edge{
        int to, next, flow;
    }edge[M];
    int first[N], sign, preEdge[N], maxFlow, surFlow[N];
    bool vis[N];
    int start, end;
    void addEdgeOneWay(int u, int v, int flow){
        edge[sign].next = first[u];
        edge[sign].flow = flow;
        edge[sign].to = v;
        first[u] = sign ++;
    }
    bool bfs(){
        memset(vis, 0, sizeof(vis));
        queue<int> que;
        que.push(start);
        surFlow[start] = INF;
        vis[start] = true;
        while(!que.empty()){
            int now = que.front();
            que.pop();
            for(int i = first[now]; i != -1; i = edge[i].next){
                int to = edge[i].to, flow = edge[i].flow;
                if(flow){
                    if(vis[to]) continue;
                    vis[to] = true;
                    surFlow[to] = min(surFlow[now], flow);
                    que.push(to);
                    preEdge[to] = i;
                    if(to == end) return true;
                }
            }
        }
        return false;
    }
    void update(){
        int now = end;
        while(now != start){
            int i = preEdge[now];
            edge[i].flow -= surFlow[end];
            edge[i^1].flow += surFlow[end];
            now = edge[i^1].to;
        }
        maxFlow += surFlow[end];
    }
public:
    Graphics(int s, int e):start(s), end(e){clear();}
    Graphics(){clear();}
    void setSandT(int s, int t){
        start = s;
        end = t;
    }
    void clear(){
        maxFlow = sign = 0;
        memset(first, 0xff, sizeof(first));
    }
    void addEdgeFlowWay(int u, int v, int flow){
        addEdgeOneWay(u, v, flow);
        addEdgeOneWay(v, u, 0);
    }
    int edmondsKarp(){
        while(bfs()) update();
        return maxFlow;
    }
};
```