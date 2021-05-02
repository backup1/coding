# Point in a Polygon

[https://open.kattis.com/problems/pointinpolygon](https://open.kattis.com/problems/pointinpolygon)

```cpp
#include <bits/stdc++.h>
using namespace std;
using ld = long double;
using pt = complex<ld>;
const ld PI = M_PI;
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int n,m;
  ld x,y;
  while(true){
    cin >> n;
    if(n == 0) break;
    vector<pt> v(n);
    ld X = -1e9;
    for(int i = 0; i < n; ++i){
      cin >> x >> y;
      v[i] = pt{x,y};
      X = max(X,x);
    }
    pt q{X+PI,0};
    cin >> m;
    while(m--){
      cin >> x >> y;
      pt p{x,y};
      ld angle = 0;
      bool bord = false;
      for(int i = 0; i < n; ++i){
        pt p1 = v[i], p2 = v[(i+1)%n];
        if(abs(abs(p1-p)+abs(p2-p)-abs(p1-p2)) < 1e-12){
          bord = true;
          break;
        }
        angle += arg((p-p1)/(p-p2));
      }
      if(bord) cout << "on\n";
      else if(abs(angle+2*PI) < 1e-12 or abs(angle-2*PI) < 1e-12) cout << "in\n";
      else cout << "out\n";
    }
  }
  return 0;
}
```

