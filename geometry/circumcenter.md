# CircumCenter

```cpp
#include <bits/stdc++.h>
using namespace std;
using pdd = pair<double, double>;

void lineFromPoints(pdd P, pdd Q, double &a, double &b, double &c){
  a = Q.second - P.second;
  b = P.first - Q.first;
  c = a*(P.first)+ b*(P.second);
}

void perpendicularBisectorFromLine(pdd P, pdd Q, double &a, double &b, double &c){
  pdd mid_point = make_pair((P.first + Q.first)/2, (P.second + Q.second)/2);
  c = -b*(mid_point.first) + a*(mid_point.second);
  double temp = a;
  a = -b;
  b = temp;
}

pdd lineLineIntersection(double a1, double b1, double c1,
                         double a2, double b2, double c2){
  double determinant = a1*b2 - a2*b1;
  if(determinant == 0) return make_pair(FLT_MAX, FLT_MAX);
  double x = (b2*c1 - b1*c2)/determinant;
  double y = (a1*c2 - a2*c1)/determinant;
  return make_pair(x, y);
}

pdd findCircumCenter(pdd P, pdd Q, pdd R){
  double a, b, c;
  lineFromPoints(P, Q, a, b, c);
  double e, f, g;
  lineFromPoints(Q, R, e, f, g);
  perpendicularBisectorFromLine(P, Q, a, b, c);
  perpendicularBisectorFromLine(Q, R, e, f, g);
  pdd circumcenter = lineLineIntersection(a, b, c, e, f, g);
  return circumcenter;
}

int main()
{
  int n;
  cin >> n;
  vector<pdd> pt(n);
  for(int i = 0; i < n; ++i){
    cin >> pt[i].first >> pt[i].second;
  }
  if(n == 2){
    double dx = pt[1].first-pt[0].first;
    double dy = pt[1].second-pt[0].second;
    cout << sqrt(dx*dx+dy*dy)/2;
    return 0;
  }
  double ans = 1e9;
  for(int i = 0; i < n; ++i){
    for(int j = i+1; j < n; ++j){
      for(int k = j+1; k < n; ++k){
        auto p = findCircumCenter(pt[i],pt[j],pt[k]);
        if(p.first == FLT_MAX){
          double xmin = pt[i].first;
          double xmax = xmin;
          double ymin = pt[i].second;
          double ymax = ymin;
          xmin = min(xmin,pt[j].first);
          xmax = max(xmax,pt[j].first);
          ymin = min(ymin,pt[j].second);
          ymax = max(ymax,pt[j].second);
          xmin = min(xmin,pt[k].first);
          xmax = max(xmax,pt[k].first);
          ymin = min(ymin,pt[k].second);
          ymax = max(ymax,pt[k].second);
          p.first = (xmin+xmax)/2.0;
          p.second = (ymin+ymax)/2.0;
        }
        cout << "==================================\n";
        cout << p.first << "," << p.second << ",";
        double ray = 0;
        for(int ii = 0; ii < n; ++ii){
          double dx = pt[i].first-p.first;
          double dy = pt[i].second-p.second;
          ray = max(ray,sqrt(dx*dx+dy*dy));
        }
        cout << ray << endl;
        ans = min(ans,ray);
      }
    }
  }
  cout.precision(40);
  cout << fixed << ans;
  return 0;
}
```

