# Convex Hull

[https://open.kattis.com/problems/convexhull](https://open.kattis.com/problems/convexhull)

```cpp
#include <bits/stdc++.h>
using namespace std;
using pt = complex<int>;
int cross(pt p1,pt p2){
  return imag(conj(p1)*p2);
}
void solve(int n){
  vector<pt> v(n);
  int x,y;
  for(int i = 0; i < n; ++i){
    cin >> x >> y;
    v[i] = pt{x,y};
  }
  sort(begin(v),end(v),[](const pt& p1,const pt& p2){
    if(real(p1) != real(p2)) return real(p1) < real(p2);
    return imag(p1) < imag(p2);
  });
  vector<pt> up = {v[0]}, down = {v[0]};
  for(int i = 1; i < n; ++i){
    while(up.size() > 1){
      int l = up.size();
      if(cross(up[l-1]-up[l-2],v[i]-up[l-2]) >= 0) up.pop_back();
      else break;
    }
    up.emplace_back(v[i]);
    while(down.size() > 1){
      int l = down.size();
      if(cross(down[l-1]-down[l-2],v[i]-down[l-2]) <= 0) down.pop_back();
      else break;
    }
    down.emplace_back(v[i]);
  }
  down.pop_back();
  while(down.size() > 1){
    up.emplace_back(down.back());
    down.pop_back();
  }
  while(up.size() > 1){
    int l = up.size();
    if(up[l-1] == up[l-2]) up.pop_back();
    else break;
  }
  reverse(begin(up),end(up));
  while(up.size() > 1){
    int l = up.size();
    if(up[l-1] == up[l-2]) up.pop_back();
    else break;
  }
  cout << up.size() << '\n';
  for(auto& p : up){
    cout << real(p) << ' ' << imag(p) << '\n';
  }
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int n;
  while(true){
    cin >> n;
    if(n == 0) break;
    solve(n);
  }
  return 0;
}
```

