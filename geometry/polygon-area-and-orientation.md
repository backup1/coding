# Polygon Area and Orientation

[https://open.kattis.com/problems/polygonarea](https://open.kattis.com/problems/polygonarea)

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
  cout << fixed;
  cout.precision(1);
  int n;
  while(true){
    cin >> n;
    if(n == 0) break;
    ld x,y;
    vector<pt> v;
    for(int i = 0; i < n; ++i){
      cin >> x >> y;
      v.emplace_back(x,y);
    }
    ld area = 0;
    for(int i = 0; i < n; ++i){
      area += cross(v[i],v[(i+1)%n]);
    }
    if(area > 0) cout << "CCW " << area/2 << '\n';
    else cout << "CW " << abs(area)/2 << '\n';
  }
  return 0;
}
```

Convex hull + polygon area

[https://open.kattis.com/problems/wrapping](https://open.kattis.com/problems/wrapping)

```cpp
#include <bits/stdc++.h>
using namespace std;
using ld = long double;
using pt = complex<ld>;
const ld PI = M_PI;
ld cross(pt a,pt b){
  return imag(conj(a)*b);
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  cout << fixed;
  cout.precision(1);
  int n,m;
  cin >> n;
  ld x,y,w,h,angle;
  while(n--){
    cin >> m;
    vector<pt> v;
    ld inarea = 0,n1 = 1;
    for(int i = 0; i < m; ++i){
      cin >> x >> y >> w >> h >> angle;
      inarea += w*h;
      pt rot=polar(n1,-PI*angle/180);
      for(ld a : {0.5,-0.5}){
        for(ld b : {0.5,-0.5}){
          pt p = pt{x,y}+pt{a*w,b*h}*rot;
          v.emplace_back(p);
        }
      }
    }
    sort(begin(v),end(v),[](const pt& a,const pt& b){
      if(real(a) != real(b)) return real(a) < real(b);
      return imag(a) < imag(b);
    });
    vector<pt> up = {v[0]}, down = {v[0]};
    for(int i = 1; i < v.size(); ++i){
      while(up.size() > 1){
        int l = up.size();
        if(cross(v[i]-up[l-2],up[l-1]-up[l-2]) <= 0) up.pop_back();
        else break;
      }
      up.push_back(v[i]);
      while(down.size() > 1){
        int l = down.size();
        if(cross(v[i]-down[l-2],down[l-1]-down[l-2]) >= 0) down.pop_back();
        else break;
      }
      down.push_back(v[i]);
    }
    down.pop_back();
    while(!down.empty()){
      up.push_back(down.back());
      down.pop_back();
    }
    ld totarea = 0;
    for(int i = 0; i < up.size()-1; ++i) totarea += cross(up[i],up[i+1]);
    totarea = abs(totarea)/2;
    cout << 100*inarea/totarea << " %\n";
  }
  return 0;
}
```

