# Centroid Decomposition

{% embed url="https://codeforces.com/problemset/problem/342/E" %}

{% embed url="https://usaco.guide/plat/centroid?lang=cpp" %}

{% embed url="https://medium.com/carpanese/an-illustrated-introduction-to-centroid-decomposition-8c1989d53308" %}

```cpp
#pragma GCC target("sse4")
#include <bits/stdc++.h>
using namespace std;
const int inf = INT_MAX/2;
vector<vector<int>> adj,dad;
vector<int> sz,pc,dist,lvl;
vector<bool> rm;
int n,ln2,root,C;

int getSz(int node,int parent = -1){
  sz[node] = 1;
  for(int x : adj[node]){
    if(x != parent and !rm[x]) sz[node] += getSz(x,node);
  }
  return sz[node];
}

int getCentroid(int node,int wsz,int parent = 0){
  for(int x : adj[node]){
    if(x != parent and !rm[x] and sz[x] > wsz) return getCentroid(x,wsz,node);
  }
  return node;
}

int centroid(int n = root){
  int wsz = getSz(n)/2;
  int c = getCentroid(n,wsz);
  rm[c] = true;
  for(int x : adj[c]){
    if(!rm[x]){
      int cx = centroid(x);
      pc[cx] = c;
    }
  }
  return c;
}

void getDad(int node,int level = 0,int parent = -1){
  for(int x : adj[node]){
    if(x != parent){
      dad[x][0] = node;
      lvl[x] = level+1;
      getDad(x,level+1,node);
    }
  }
}

void getDads(){
  dad[root][0] = root;
  lvl[root] = 0;
  getDad(root);
  for(int i = 1; i < ln2; ++i){
    for(int node = 1; node <= n; ++node){
      int prev = dad[node][i-1];
      dad[node][i] = dad[prev][i-1];
    }
  }
}

int lca(int u,int v){
  if(lvl[v] < lvl[u]) swap(u,v);
  int d = lvl[v]-lvl[u];
  if(d > 0){
    for(int i = 0; i < ln2; ++i){
      if(d&(1<<i)){
        v = dad[v][i];
        d ^= (1<<i);
        if(d == 0) break;
      }
    }
  }
  if(u == v) return u;
  for(int i = ln2-1; i >= 0; --i){
    if(dad[u][i] != dad[v][i]){
      u = dad[u][i];
      v = dad[v][i];
    }
  }
  return dad[u][0];
}

void update(int node){
  dist[node] = 0;
  int cn = node,lvln = lvl[node];
  do{
    cn = pc[cn];
    dist[cn] = min(dist[cn],lvln+lvl[cn]-2*lvl[lca(node,cn)]);
  } while(cn != C);
}

void query(int node){
  int ret = dist[node];
  int cn = node,lvln = lvl[node];
  do{
    cn = pc[cn];
    ret = min(ret,lvln+lvl[cn]-2*lvl[lca(node,cn)]+dist[cn]);
  } while(cn != C);
  cout << ret << '\n';
}

int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int m,t,u,v;
  cin >> n >> m;
  ln2 = log2(n)+1;
  adj.resize(n+1);
  dad.resize(n+1,vector<int>(ln2+1,root));
  sz.resize(n+1);
  rm.resize(n+1);
  pc.resize(n+1,inf);
  lvl.resize(n+1);
  dist.resize(n+1,inf);
  for(int i = 1; i < n; ++i){
    cin >> u >> v;
    adj[u].emplace_back(v);
    adj[v].emplace_back(u);
  }
  root = 1;
  getDads();
  C = centroid();
  pc[C] = C;
  update(1);
  for(int i = 0; i < m; ++i){
    cin >> t >> v;
    if(t == 1) update(v);
    else query(v);
  }
  return 0;
}
```







