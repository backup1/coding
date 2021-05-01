# Line Segment Intersection

[https://open.kattis.com/problems/segmentintersection](https://open.kattis.com/problems/segmentintersection)

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
  cout.precision(2);
  int n;
  cin >> n;
  ld x1,y1,x2,y2,x3,y3,x4,y4;
  for(int i = 0; i < n; ++i){
    cin >> x1 >> y1 >> x2 >> y2 >> x3 >> y3 >> x4 >> y4;
    pt a1{x1,y1}, a2{x2,y2}, b1{x3,y3}, b2{x4,y4};
    if(cross(a1-a2,b1-b2) != 0){
      if(cross(a1-b1,a2-b1)*cross(a1-b2,a2-b2) <= 0 and cross(b1-a1,b2-a1)*cross(b1-a2,b2-a2) <= 0){
        ld t = cross(a1-b1,a1-b2)/cross(a1-a2,b1-b2);
        pt p = a1+t*(a2-a1);
        cout << real(p) << ' ' << imag(p) << '\n';
      }
      else cout << "none\n";
    }
    else if(cross(a1-b1,a1-b2) == 0){
      pair<ld,ld> p1(x1,y1),p2(x2,y2),p3(x3,y3),p4(x4,y4);
      if(p1 > p2) swap(p1,p2);
      if(p3 > p4) swap(p3,p4);
      if(p2 == p3) cout << p2.first << ' ' << p2.second << '\n';
      else if(p4 == p1) cout << p1.first << ' ' << p1.second << '\n';
      else{
        vector<pair<ld,ld>> v={p1,p2,p3,p4};
        sort(begin(v),end(v));
        if(v[1] == v[2]) cout << v[1].first << ' ' << v[1].second << '\n';
        else if(v[1] == p2 or v[1] == p4) cout << "none\n";
        else cout << v[1].first << ' ' << v[1].second << ' ' << v[2].first << ' ' << v[2].second << '\n';
      }
    }
    else cout << "none\n";
  }
  return 0;
}
```

