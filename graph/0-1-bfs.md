# 0-1 BFS

In some particular case, there is a more efficient algorithm than Dijkstra \(in O\(V+ElnE\) \) : the 0-1 BFS \(in O\(V+E\) \)

Blog entry: [https://codeforces.com/blog/entry/22276](https://codeforces.com/blog/entry/22276)

## Questions

{% embed url="https://codeforces.com/problemset/problem/1063/B" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
const int inf = INT_MAX;
struct pt{
  int x,y;
};
vector<pt> dirs = { {0,1}, {0,-1}, {1,0}, {-1,0} };
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  int n,m,r,c,L,R;
  cin >> n >> m >> r >> c >> L >> R;
  vector<string> vs(n);
  for(int i = 0; i < n; ++i) cin >> vs[i];
  vector<vector<int>> left(n,vector<int>(m,inf));
  deque<pt> q;
  q.push_front(pt{r-1,c-1});
  left[r-1][c-1] = 0;
  while(!q.empty()){
    pt p = q.front();
    q.pop_front();
    int pleft = left[p.x][p.y];
    if(pleft > L) break;
    for(auto dir : dirs){
      int x = p.x + dir.x;
      int y = p.y + dir.y;
      if(0 <= x and x < n and 0 <= y and y < m and vs[x][y] == '.'){
        int nleft = pleft;
        if(dir.y == -1) ++nleft;
        if(nleft < left[x][y]){
          left[x][y] = nleft;
          if(nleft == pleft+1) q.push_back(pt{x,y});
          else if(nleft == pleft) q.push_front(pt{x,y});
        }
      }
    }
  }
  int ans = 0;
  for(int i = 0; i < n; ++i){
    for(int j = 0; j < m; ++j){
      if(vs[i][j] == '.' and left[i][j] <= L and left[i][j]+j-(c-1) <= R) ++ans;
    }
  }
  cout << ans;
  return 0;
}
```

