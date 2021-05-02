# Line Segment Distance

[https://open.kattis.com/problems/segmentdistance](https://open.kattis.com/problems/segmentdistance)

```cpp
#include <bits/stdc++.h>
using namespace std;
using ld = long double;
using pt = complex<ld>;
int sgn(ld a){
  if(abs(a) < 1e-12) return 0;
  if(a > 0) return 1;
  return -1;
}
ld dot(pt a,pt b){
  return real(conj(a)*b);
}
ld cross(pt a,pt b){
  return imag(conj(a)*b);
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  cout << fixed;
  cout.precision(2);
  int n;
  cin >> n;
  while(n--){
    ld x,y;
    cin >> x >> y;
    pt a{x,y};
    cin >> x >> y;
    pt b{x,y};
    cin >> x >> y;
    pt c{x,y};
    cin >> x >> y;
    pt d{x,y};
    if(sgn(cross(b-a,c-a))*sgn(cross(b-a,d-a)) < 0 and sgn(cross(d-c,a-c))*sgn(cross(d-c,b-c)) < 0){
      cout << "0.00\n";
      continue;
    }
    ld dist = min({abs(a-c),abs(a-d),abs(b-c),abs(b-d)});
    for(pt p : {a,b}){
      if(sgn(dot(p-c,d-c))*sgn(dot(p-d,c-d)) > 0){
        ld area2 = abs(cross(p,c)+cross(c,d)+cross(d,p));
        dist = min(dist,area2/abs(c-d));
      }
    }
    for(pt p : {c,d}){
      if(sgn(dot(p-a,b-a))*sgn(dot(p-b,a-b)) > 0){
        ld area2 = abs(cross(p,a)+cross(a,b)+cross(b,p));
        dist = min(dist,area2/abs(a-b));
      }
    }
    cout << dist << '\n';
  }
  return 0;
}
```

