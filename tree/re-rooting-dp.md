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

{% embed url="https://codeforces.com/problemset/problem/1324/F" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
const int inf = (INT_MAX>>1);
void dfs(int node,int pere,vector<vector<int>>& adj,vector<int>& maxv){
  for(int i : adj[node]){
    if(i == pere) continue;
    dfs(i,node,adj,maxv);
    if(maxv[i] > 0) maxv[node] += maxv[i];
  }
}
void dfs2(int node,int pere,vector<vector<int>>& adj,vector<int>& maxv,vector<int>& ans){
  int maxvnode = maxv[node];
  for(int i : adj[node]){
    if(i == pere) continue;
    maxv[node] = maxvnode;
    if(maxv[i] > 0) maxv[node] -= maxv[i];
    if(maxv[node] > 0) maxv[i] += maxv[node];
    ans[i] = maxv[i];
    dfs2(i,node,adj,maxv,ans);
  }
}
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  int n,a,u,v;
  cin >> n;
  vector<int> maxv(n+1),ans(n+1,-inf);
  for(int i = 1; i <= n; ++i){
    cin >> a;
    if(a == 1) maxv[i] = 1;
    else maxv[i] = -1;
  }
  vector<vector<int>> adj(n+1);
  for(int i = 1; i < n; ++i){
    cin >> u >> v;
    adj[u].push_back(v);
    adj[v].push_back(u);
  }
  dfs(1,-1,adj,maxv);
  ans[1] = maxv[1];
  dfs2(1,-1,adj,maxv,ans);
  for(int i = 1; i <= n; ++i) cout << ans[i] << ' ';
  return 0;
}
```

