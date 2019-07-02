# Re-rooting \(DP\)

## Questions

{% embed url="https://codeforces.com/contest/1187/problem/E" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
vector<vector<ll>> adj;
vector<ll> sz,nb;
ll ans = 0;
void init_sz(ll node,ll pere = -1){
  sz[node] = 1;
  for(int i : adj[node]){
    if(i == pere) continue;
    init_sz(i,node);
    sz[node] += sz[i];
  }
}
void init_nb(ll node,ll pere = -1){
  nb[node] = sz[node];
  for(int i : adj[node]){
    if(i == pere) continue;
    init_nb(i,node);
    nb[node] += nb[i];
  }
}
void dfs(ll node,ll pere = -1){
  ans = max(ans,nb[node]);
  for(ll i : adj[node]){
    if(i == pere) continue;
    nb[node] -= nb[i];
    nb[node] -= sz[i];
    sz[node] -= sz[i];
    nb[i] += nb[node];
    nb[i] += sz[node];
    sz[i] += sz[node];
    dfs(i,node);
    nb[i] -= nb[node];
    nb[i] -= sz[node];
    sz[i] -= sz[node];
    nb[node] += nb[i];
    nb[node] += sz[i];
    sz[node] += sz[i];
  }
}
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  ll n,u,v;
  cin >> n;
  adj = vector<vector<ll>>(n);
  sz = vector<ll>(n);
  nb = vector<ll>(n);
  for(ll i = 1; i < n; ++i){
    cin >> u >> v;
    adj[u-1].push_back(v-1);
    adj[v-1].push_back(u-1);
  }
  init_sz(0);
  init_nb(0);
  dfs(0);
  cout << ans;
  return 0;
}
```



