# DFS tree

## Questions

{% embed url="https://codeforces.com/problemset/problem/377/A" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
int n,m;
vector<string> vs;
vector<pair<int,int>> dirs = { {0,1},{0,-1},{1,0},{-1,0} };
void dfs(int x,int y,int& k,vector<vector<bool>>& vu){
  if(k == 0 or vu[x][y]) return;
  vu[x][y] = true;
  for(auto dir : dirs){
    int x1 = x+dir.first, y1 = y+dir.second;
    if(0 <= x1 and x1 < n and 0 <= y1 and y1 < m){
      if(vs[x1][y1] == '.') dfs(x1,y1,k,vu);
    }
  }
  if(k > 0){
    vs[x][y] = 'X';
    --k;
  }
}
void draw(){
  for(auto& s : vs) cout << s << '\n';
}
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  int k;
  cin >> n >> m >> k;
  vs = vector<string>(n);
  for(int i = 0; i < n; ++i) cin >> vs[i];
  for(int i = 0; i < n; ++i){
    for(int j = 0; j < m; ++j){
      if(vs[i][j] == '.'){
        vector<vector<bool>> vu(n,vector<bool>(m));
        dfs(i,j,k,vu);
        draw();
        return 0;
      }
    }
  }
  return 0;
}
```

