---
description: Forgot in which context this implementation was used. Not sure if working.
---

# \[TODO\] Ford Fulkerson

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const ll inf = LLONG_MAX/2;
const ll max_n = 501;
vector<vector<int>> adj(max_n);
vector<vector<bool>> vu(max_n,vector<bool>(max_n));
int k,n,m;
vector<int> ans;
void dfs(int node,bool& ok){
 if (node == n){
 ok = true;
 return;
 }
 if (ok) return;
 for (auto i:adj[node]){
 if (!vu[node][i]){
 ans.emplace_back(i);
 vu[node][i] = true;
 dfs(i,ok);
 if (ok) return;
 ans.pop_back();
 }
 if (ok) return;
 }
}
int main(){
 ios::sync_with_stdio(false);
 cin.tie(nullptr);
 cin >> n >> m >> k;
 for (int i=0; i<m; ++i){
 int a,b;
 cin >> a >> b;
 adj[a].emplace_back(b);
 sort(adj[a].rbegin(),adj[a].rend());
 }
 for (int i=0; i<k; ++i){
 bool ok = false;
 dfs(1,ok);
 cout << ans.size()+1 << '\n' << 1 << '\n';
 for (auto j:ans) cout << j << '\n';
 ans.clear();
 }
 return 0;
}

```

