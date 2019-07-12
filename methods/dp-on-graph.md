# DP on Graph

## Questions

This question is using DP on graph with a very limited CPU time and memory resource. Be careful.

{% embed url="https://codeforces.com/problemset/problem/721/C" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
const int inf = 1e9+5;
vector<vector<pair<int,int>>> pre;
vector<map<int,int>> from;
vector<vector<int>> vt;
// vt[i][j] min time from 1 to i by visiting j
inline int check(int node,int l){
  if(node == 1){
    if(l == 1) return 0;
    return inf;
  }
  if(l < 2) return inf;
  if(vt[node][l] == -1){
    vt[node][l] = inf;
    for(auto& p : pre[node]){
      int t = check(p.first,l-1);
      t += p.second;
      if(t < vt[node][l]){
        vt[node][l] = t;
        from[node][l] = p.first;
      }
    }
  }
  return vt[node][l];
}
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  int n,m,T,u,v,t;
  cin >> n >> m >> T;
  pre = vector<vector<pair<int,int>>>(n+1);
  for(int i = 0; i < m; ++i){
    cin >> u >> v >> t;
    pre[v].emplace_back(u,t);
  }
  int ans = 0;
  from = vector<map<int,int>>(n+1);
  vt = vector<vector<int>>(n+1,vector<int>(n+1,-1));
  vt[1][1] = 0;
  for(int l = n; l > 0; --l){
    if(check(n,l) <= T){
      ans = l;
      break;
    }
  }
  cout << ans << '\n';
  vector<int> path;
  int node,prev = n,len = ans;
  do{
    node = prev;
    path.push_back(node);
    prev = from[node][len];
    --len;
  } while(node != 1);
  reverse(begin(path),end(path));
  for(int i : path) cout << i << ' ';
  return 0;
}
```

