# Detect Cycle and Output Loop

{% embed url="https://codeforces.com/problemset/problem/131/D" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
int n;
bool dfs(int node,int pere,vector<vector<int>>& adj,vector<bool>& vu,vector<bool>& path,deque<int>& loop){
  vu[node] = true;
  path[node] = true;
  loop.push_back(node);
  for(int i : adj[node]){
    if(i == pere) continue;
    if(path[i]){
      loop.push_back(i);
      return true;
    }
    if(vu[i]) return false;
    if(dfs(i,node,adj,vu,path,loop)) return true;
  }
  loop.pop_back();
  path[node] = false;
  return false;
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int a,b;
  cin >> n;
  vector<vector<int>> adj(n+1);
  for(int i = 0; i < n; ++i){
    cin >> a >> b;
    adj[a].push_back(b);
    adj[b].push_back(a);
  }
  deque<int> loop;
  vector<bool> vu(n+1),path(n+1);
  dfs(1,0,adj,vu,path,loop);
  while(!loop.empty() and loop.front() != loop.back()) loop.pop_front();
  queue<int> q;
  vector<int> len(n+1,-1);
  for(int i : loop){
    len[i] = 0;
    q.emplace(i);
  }
  while(!q.empty()){
    int i = q.front();
    q.pop();
    for(int j : adj[i]){
      if(len[j] == -1){
        len[j] = len[i]+1;
        q.emplace(j);
      }
    }
  }
  for(int i = 1; i <= n; ++i) cout << len[i] << ' ';
  return 0;
}
```

