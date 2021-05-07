# DP on tree

[https://codeforces.com/problemset/problem/274/B](https://codeforces.com/problemset/problem/274/B)

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = int64_t;
const ll mod = 1e9+7;
const int N = 1e5+5;
int n;
ll val[N],up[N],down[N];
vector<int> adj[N];
void dfs(int node,int pere){
  for(int i : adj[node]){
    if(i == pere) continue;
    dfs(i,node);
    up[node] = max(up[i],up[node]);
    down[node] = max(down[i],down[node]);
  }
  ll cval = val[node]+up[node]-down[node];
  if(cval < 0) up[node] += -cval;
  else down[node] += cval;
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  memset(up,0,sizeof(up));
  memset(down,0,sizeof(down));
  cin >> n;
  for(int i = 1; i < n; ++i){
    int a,b;
    cin >> a >> b;
    adj[a].push_back(b);
    adj[b].push_back(a);
  }
  for(int i = 1; i <= n; ++i) cin >> val[i];
  dfs(1,0);
  cout << up[1]+down[1];
  return 0;
}
```

