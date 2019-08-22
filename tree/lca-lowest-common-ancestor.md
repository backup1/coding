# LCA \(Lowest Common Ancestor\)

Binary lifting is in use to make the algorithm in O\(logN\)

{% embed url="https://www.spoj.com/problems/QTREE2/" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 10005;
int pos,logN;
vector<vector<int>> parent;
vector<int> adj[N],cost[N],tcost(N),depth(N);
inline void dfs(int node,int pere,int tc,int dep){
  parent[0][node] = pere;
  tcost[node] = tc;
  depth[node] = dep;
  for(int i = 0; i < adj[node].size(); ++i){
    if(adj[node][i] == pere) continue;
    dfs(adj[node][i],node,tc+cost[node][i],dep+1);
  }
}
inline int LCA(int u,int v){
  if(depth[u] < depth[v]) swap(u,v);
  int d = depth[u]-depth[v];
  for(int i = 0; i < logN; ++i){
    if(d&(1<<i)){
      u = parent[i][u];
      d ^= 1<<i;
      if(d == 0) break;
    }
  }
  if(u == v) return u;
  for(int i = logN-1; i >= 0; --i){
    if(parent[i][u] != parent[i][v]){
      u = parent[i][u];
      v = parent[i][v];
    }
  }
  return parent[0][u];
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int t;
  cin >> t;
  while(t--){
    int n,u,v,c,k;
    cin >> n;
    pos = 0;
    logN = int(log2(n))+1;
    parent = vector<vector<int>>(logN,vector<int>(n,-1));
    for(int i = 0; i < n; ++i){
      adj[i].clear();
      cost[i].clear();
    }
    for(int i = 0; i < n-1; ++i){
      cin >> u >> v >> c;
      --u;
      --v;
      adj[u].push_back(v);
      cost[u].push_back(c);
      adj[v].push_back(u);
      cost[v].push_back(c);
    }
    string cmd;
    dfs(0,-1,0,0);
    // DP to prepare LCA
    for(int i = 1; i < logN; ++i){
      for(int j = 0; j < n; ++j){
        int prev = parent[i-1][j];
        if(prev >= 0) parent[i][j] = parent[i-1][prev];
      }
    }
    while(true){
      cin >> cmd;
      if(cmd[1] == 'O') break;
      cin >> u >> v;
      --u;
      --v;
      int lca = LCA(u,v);
      if(cmd[0] == 'D'){
        cout << tcost[u]+tcost[v]-2*tcost[lca] << '\n';
      }
      else{
        cin >> k;
        int ulca = depth[u]-depth[lca],ans;
        if(k == ulca+1) ans = lca;
        else{
          if(k <= ulca){
            --k;
            ans = u;
            for(int i = 0; i <= logN; ++i){
              if(k&(1<<i)){
                ans = parent[i][ans];
                k ^= 1<<i;
                if(k == 0) break;
              }
            }
          }
          else{
            k = depth[u]+depth[v]-2*depth[lca]+1-k;
            ans = v;
            for(int i = 0; i <= logN; ++i){
              if(k&(1<<i)){
                ans = parent[i][ans];
                k ^= 1<<i;
                if(k == 0) break;
              }
            }
          }
        }
        cout << ans+1 << '\n';
      }
    }
  }
  return 0;
}
```

