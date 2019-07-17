# Bitset

## Questions

{% embed url="https://codeforces.com/contest/1194/problem/E" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
bool check(pair<int,pair<int,int>>& v1,
           pair<int,pair<int,int>>& h1){
  return (h1.second.first <= v1.first and
          v1.first <= h1.second.second and
          v1.second.first <= h1.first and
          h1.first <= v1.second.second);
}
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  int n,x1,x2,y1,y2;
  cin >> n;
  vector<pair<int,pair<int,int>>> v,h;
  for(int i = 0; i < n; ++i){
    cin >> x1 >> y1 >> x2 >> y2;
    if(x1 == x2) h.push_back({x1,{min(y1,y2),max(y1,y2)}});
    else v.push_back({y1,{min(x1,x2),max(x1,x2)}});
  }
  if(v.size() > h.size()) swap(v,h);
  int vsz = v.size();
  vector<bitset<5000>> vb(vsz);
  for(int i = 0; i < vsz; ++i){
    for(int j = 0; j < h.size(); ++j){
      if(check(v[i],h[j])) vb[i][j] = 1;
    }
  }
  ll ans = 0;
  for(int i = 0; i < vsz; ++i){
    for(int j = i+1; j < vsz; ++j){
      int tmp = (vb[i]&vb[j]).count();
      ans += tmp*(tmp-1)/2;
    }
  }
  cout << ans;
  return 0;
}
```

