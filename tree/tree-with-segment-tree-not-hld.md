# Tree with Segment Tree \(not HLD\)

{% embed url="https://www.codechef.com/AUG19B/problems/CHGORAM" %}

My idea of this problem comes from **tomsyd**. Thanks a lot !

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const int N = 1e5;
int n,p1,p2,p3;
ll ans;
vector<int> t;
void update(int p){
  for(t[p += n] = 1; p > 1; p >>= 1) t[p>>1] = t[p] + t[p^1];
}
int query(int l,int r){ // [l,r)
  int res = 0;
  for(l += n,r += n; l < r; l >>= 1,r >>= 1){
    if(l&1) res += t[l++];
    if(r&1) res += t[--r];
  }
  return res;
}
inline void dfs(int node,int pere,vector<vector<int>>& adj){
  vector<ll> low,high;
  ll totl = node-1,toth = n-node;
  update(node-1);
  ll low_old = query(0,node),high_old = query(node,n);
  for(int i : adj[node]){
    if(i == pere) continue;
    dfs(i,node,adj);
    ll low_new = query(0,node),high_new = query(node,n);
    ll low_delta = low_new-low_old,high_delta = high_new-high_old;
    totl -= low_delta;
    toth -= high_delta;
    low.emplace_back(low_delta);
    high.emplace_back(high_delta);
    low_old = low_new;
    high_old = high_new;
  }
  low.emplace_back(totl);
  high.emplace_back(toth);
  if(p2 == 3){
    ll tot = node-1;
    for(ll i : low){
      tot -= i;
      ans += i*tot;
    }
  }
  else if(p2 == 1){
    ll tot = n-node;
    for(ll i : high){
      tot -= i;
      ans += i*tot;
    }
  }
  else{
    toth = n-node;
    for(int i = 0; i < low.size(); ++i) ans += low[i]*(toth-high[i]);
  }
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int T,u,v;
  cin >> T;
  while(T--){
    cin >> n >> p1 >> p2 >> p3;
    vector<vector<int>> adj(n+1);
    for(int i = 1; i < n; ++i){
      cin >> u >> v;
      adj[u].emplace_back(v);
      adj[v].emplace_back(u);
    }
    ans = 0;
    t = vector<int>(2*n);
    dfs(1,-1,adj);
    cout << ans << '\n';
  }
  return 0;
}
```

