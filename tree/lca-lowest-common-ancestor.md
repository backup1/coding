# LCA \(Lowest Common Ancestor\)

Binary lifting is in use to make the algorithm in O\(logN\)

Ref. [https://cp-algorithms.com/graph/lca\_binary\_lifting.html](https://cp-algorithms.com/graph/lca_binary_lifting.html)

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

{% embed url="https://www.spoj.com/problems/LCA/" %}

```cpp
Required time for various technique using fast I/O:
1. Naive Approach - preprocess: O(N), Query: O(N^2)-------------------------0.23s
2. using Square Root Decomposition: preprocess: O(N), Query: O(sqrt(N))-----0.04s
3. using Segment Tree: preprocess: O(NlogN), Query: O(logN)-----------------0.02s
4. using Sparse Table: preprocess: O(NlogN), Query: O(1)--------------------0.04s
5. Binary Lifting: preprocess: O(N), Query: O(logN)-------------------------0.03s
6. Farach Colton and Bender Algorithm: preprocess: O(N), Query: O(1)--------0.03s
7. Tarjan's Offline Algorithm: preprocess: O(N), Query: O(l1)---------------0.03s
```

{% tabs %}
{% tab title="naive 0.25s" %}
```cpp
#include <bits/stdc++.h>
using namespace std;
inline void dfs(int node,int pere,vector<vector<int>>& adj,vector<int>& level){
  for(int i : adj[node]){
    if(i == pere) continue;
    level[i] = level[node] + 1;
    dfs(i,node,adj,level);
  }
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int t;
  cin >> t;
  for(int tt = 1; tt <= t; ++tt){
    cout << "Case " << tt << ":\n";
    int n,m,a,b;
    cin >> n;
    vector<int> pere(n+1,-1),level(n+1);
    vector<vector<int>> adj(n+1);
    for(int i = 1; i <= n; ++i){
      cin >> m;
      for(int j = 0; j < m; ++j){
        cin >> a;
        pere[a] = i;
        adj[i].push_back(a);
      }
    }
    int root;
    for(int i = 1; i <= n; ++i){
      if(pere[i] == -1){
        root = i;
        break;
      }
    }
    level[root] = 1;
    dfs(root,-1,adj,level);
    int q;
    cin >> q;
    while(q--){
      cin >> a >> b;
      while(level[a] > level[b]) a = pere[a];
      while(level[b] > level[a]) b = pere[b];
      while(a != b) a = pere[a], b = pere[b];
      cout << a << '\n';
    }
  }
  return 0;
}
```
{% endtab %}

{% tab title="binary lifting 0.04s" %}
```cpp
#include <bits/stdc++.h>
using namespace std;
inline void dfs(int node,int pere,vector<vector<int>>& adj,vector<int>& level){
  for(int i : adj[node]){
    if(i == pere) continue;
    level[i] = level[node] + 1;
    dfs(i,node,adj,level);
  }
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int t;
  cin >> t;
  for(int tt = 1; tt <= t; ++tt){
    cout << "Case " << tt << ":\n";
    int n,m,a,b;
    cin >> n;
    vector<int> level(n+1);
    vector<vector<int>> pere(10,vector<int>(n+1));
    vector<vector<int>> adj(n+1);
    for(int i = 1; i <= n; ++i){
      cin >> m;
      for(int j = 0; j < m; ++j){
        cin >> a;
        pere[0][a] = i;
        adj[i].push_back(a);
        adj[a].push_back(i); // missing in my submission !!!
      }
    }
    for(int i = 1; i < 10; ++i){
      for(int j = 1; j <= n; ++j){
        pere[i][j] = pere[i-1][pere[i-1][j]];
      }
    }
    int root;
    for(int i = 1; i <= n; ++i){
      if(pere[0][i] == 0){
        root = i;
        break;
      }
    }
    level[root] = 1;
    dfs(root,-1,adj,level);
    int q;
    cin >> q;
    while(q--){
      cin >> a >> b;
      if(level[a] > level[b]){ // no need to use while
        int d = level[a]-level[b];
        for(int i = 9; i >= 0; --i){
          if(d&(1<<i)) a = pere[i][a];
        }
      }
      else if(level[b] > level[a]){ // no need to use while
        int d = level[b]-level[a];
        for(int i = 9; i >= 0; --i){
          if(d&(1<<i)) b = pere[i][b];
        }
      }
      if(a == b) cout << a << '\n';
      else{
        for(int i = 9; i >= 0; --i){
          if((1<<i) > level[a]) continue;
          if(pere[i][a] != pere[i][b]){
            a = pere[i][a];
            b = pere[i][b];
          }
        }
        cout << pere[0][a] << '\n';
      }
    }
  }
  return 0;
}
```
{% endtab %}
{% endtabs %}

