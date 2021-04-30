# Rotating calipers

[https://open.kattis.com/problems/roberthood](https://open.kattis.com/problems/roberthood)

```cpp
#include <bits/stdc++.h>
using namespace std;
using ld = long double;
using pt = complex<ld>;
ld cross(pt p1,pt p2){
  return imag(conj(p1)*p2);
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int n;
  cin >> n;
  vector<pt> v(n);
  ld x,y;
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
  int l = up.size();
  ld dist = 0;
  if(l <= 1000){
    for(int i = 0; i < l; ++i){
      for(int j = i+1; j < l; ++j) dist = max(dist,abs(up[j]-up[i]));
    }
  }
  else{
    int idx = 1;
    for(int i = 0; i < l; ++i){
      pt a = up[i], b = up[(i+1)%l];
      while(true){
        pt p = up[idx];
        dist = max({dist,abs(p-a),abs(p-b)});
        if(abs(p-b) >= abs(p-a)) break;
        idx = (idx+1)%l;
      }
    }
  }
  cout << fixed;
  cout.precision(9);
  cout << dist;
  return 0;
}
```

