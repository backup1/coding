# Implicit Graph

## Questions on sub-sequences

{% embed url="https://codeforces.com/problemset/problem/463/D" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
int dfs(int node,vector<vector<int>>& adj,vector<int>& ans){
  if(ans[node] == 0){
    ans[node] = 1;
    for(int i : adj[node]) ans[node] = max(ans[node],dfs(i,adj,ans)+1);
  }
  return ans[node];
}
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  int n,k,a;
  cin >> n >> k;
  vector<map<int,int>> vm(k);
  for(int i = 0; i < k; ++i){
    for(int j = 0; j < n; ++j){
      cin >> a;
      vm[i][a] = j;
    }
  }
  vector<vector<int>> adj(n+1);
  for(int i = 1; i <= n; ++i){
    for(int j = 1; j <= n; ++j){
      bool ok = true;
      for(int h = 0; h < k; ++h){
        if(vm[h][i] >= vm[h][j]){
          ok = false;
          break;
        }
      }
      if(ok) adj[i].push_back(j);
    }
  }
  vector<int> ans(n+1);
  for(int i = 1; i <= n; ++i) dfs(i,adj,ans);
  cout << *max_element(begin(ans),end(ans));
  return 0;
}
```

## Questions on strings

{% embed url="https://atcoder.jp/contests/abc135/tasks/abc135\_f" %}



