# Dynamic Tree Diameter

Tree Diameter \[editorial with 3 methods\] : [https://www.cnblogs.com/TinyWong/p/11260601.html](https://www.cnblogs.com/TinyWong/p/11260601.html)

Tree Diameter \[very good editorial for the trick solution , in chinese\] : [https://zhuanlan.zhihu.com/p/84236967](https://zhuanlan.zhihu.com/p/84236967)

Question link : [https://dmoj.ca/problem/ceoi19p2](https://dmoj.ca/problem/ceoi19p2)

Solution with comments

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const int SZ = 1e5+5;
int N, Q, num;
int X[SZ], L[SZ], R[SZ];
ll W[SZ], Wmod, ans;
vector<pair<int,int>> adj[SZ];

// D = distance to root
// Dist(a,b) = D_a - 2*D_lca + D_b (lca is between a and b in DFS tree)
// M (Middle) = max(-2*D_lca)
// LM (Left-Middle) = max(D_a-2*D_lca)
// MR (Middle-Right) = max(-2*D_lca+D_b)
// LMR (Left-Middle-Right) = max(D_a-2*D_lca+D_b)
struct Node{
  ll D = 0, M = 0, LM = 0, MR = 0, LMR = 0, lazy = 0;
  void operator+=(const ll& val){
    D += val; // Distance from the node to root is increased by val
    lazy += val; // Lazy propagation
    M -= 2*val;
    LM -= val; // both D_a and D_lca are incresed by val
    MR -= val; // idem for D_lca and D_b
    // LMR unchanged
  }
  Node operator+(const Node& r) const {
    Node node;
    node.D = max(D, r.D);
    node.M = max(M, r.M);
    node.LM = max({LM, r.LM, D+r.M});
    node.MR = max({MR, r.MR, M+r.D});
    node.LMR = max({LMR, r.LMR, LM+r.D, D+r.MR});
    return node;
  }
} T[8*SZ];

void update(int idx, int s, int e, int ts, int te, ll val){
  if(te < s or e < ts) return;
  if(ts <= s and e <= te){
    T[idx] += val;
    return;
  }
  T[2*idx] += T[idx].lazy;
  T[2*idx+1] += T[idx].lazy;
  T[idx].lazy = 0;
  int mid = (s+e)/2;
  update(2*idx, s, mid, ts, te, val);
  update(2*idx+1, mid+1, e, ts, te, val);
  T[idx] = T[2*idx] + T[2*idx+1];
}

void dfs(int u, int p){
  L[u] = ++num;
  for(auto& item : adj[u]){
    int x = item.first, idx = item.second;
    if(x == p) continue;
    X[idx] = x;
    dfs(x,u);
    ++num;
  }
  R[u] = num;
}

int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  cin >> N >> Q >> Wmod;
  for(int i = 1; i < N; ++i){
    int u, v;
    cin >> u >> v >> W[i];
    adj[u].emplace_back(v, i);
    adj[v].emplace_back(u, i);
  }
  dfs(1, 0);
  for(int i = 1; i < N; ++i) update(1, 1, 2*N, L[X[i]], R[X[i]], W[i]);
  while(Q--){
    ll k,w;
    cin >> k >> w;
    k = (k + ans) % (N-1) + 1;
    w = (w + ans) % Wmod;
    update(1, 1, 2*N, L[X[k]], R[X[k]], w-W[k]);
    W[k] = w;
    ans = T[1].LMR;
    cout << ans << '\n';
  }
  return 0;
}
```

