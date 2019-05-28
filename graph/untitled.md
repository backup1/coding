# Flood Fill

Questions :

* [https://atcoder.jp/contests/agc033/tasks/agc033\_a](https://atcoder.jp/contests/agc033/tasks/agc033_a)

```cpp
#include <bits/stdc++.h>
using namespace std;

class floodfill{
  struct pt{
    int x,y,n;
  };
  vector<pt> dirs = { {0,1,1}, {0,-1,1}, {1,0,1}, {-1,0,1} };
  int h,w;
  vector<string> vs;
public:
  floodfill(int _h,int _w,vector<string>& _vs) : h(_h),w(_w),vs(_vs) {}
  int dist(){
    int ans = 0;
    queue<pt> q;
    for(int i = 0; i < h; ++i){
      for(int j = 0; j < w; ++j){
        if(vs[i][j] == '#'){
          vs[i][j] = true;
          q.emplace(pt{i,j,0});
        }
      }
    }
    while(!q.empty()){
      auto p = q.front();
      q.pop();
      for(auto& d : dirs){
        int x=p.x+d.x,y=p.y+d.y,n=p.n+d.n;
        if(0 <= x and x < h and 0 <= y and y < w){
          if(vs[x][y] == '.'){
            vs[x][y] = '#';
            ans = max(ans,n);
            q.emplace(pt{x,y,n});
          }
        }
      }
    }
    return ans;
  }
};

int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int h,w,nb = 0;
  cin >> h >> w;
  vector<string> vs(h);
  for(int i = 0; i < h; ++i) cin >> vs[i];
  floodfill ff(h,w,vs);
  cout << ff.dist();
  return 0;
}
```

```cpp
#include <bits/stdc++.h>
using namespace std;
struct pt{
  int x,y,n;
};
vector<pt> dirs = { {0,1,1}, {0,-1,1}, {1,0,1}, {-1,0,1} };
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int h,w,nb = 0;
  cin >> h >> w;
  vector<string> vs(h);
  queue<pt> q;
  for(int i = 0; i < h; ++i){
    cin >> vs[i];
    for(int j = 0; j < w; ++j){
      if(vs[i][j] == '#'){
        vs[i][j] = true;
        q.emplace(pt{i,j,0});
      }
    }
  }
  while(!q.empty()){
    auto p = q.front();
    q.pop();
    for(auto& d : dirs){
      int x=p.x+d.x,y=p.y+d.y,n=p.n+d.n;
      if(0 <= x and x < h and 0 <= y and y < w){
        if(vs[x][y] == '.'){
          vs[x][y] = '#';
          nb = max(nb,n);
          q.emplace(pt{x,y,n});
        }
      }
    }
  }
  cout << nb;
  return 0;
}
```

