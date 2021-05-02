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

