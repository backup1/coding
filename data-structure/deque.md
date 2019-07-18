# Deque

## Questions

{% embed url="https://codeforces.com/contest/1195/problem/E" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  ll n,m,a,b;
  cin >> n >> m >> a >> b;
  ll x,y,z;
  vector<ll> g(n*m);
  vector<vector<ll>> h(n+1,vector<ll>(m+1)),hmin(n+1,vector<ll>(m+1));
  cin >> g[0] >> x >> y >> z;
  for(ll i = 1; i < n*m; ++i) g[i] = (g[i-1]*x+y)%z;
  for(ll i = 1; i <= n; ++i){
    for(ll j = 1; j <= m; ++j){
      h[i][j] = g[(i-1)*m+j-1];
    }
  }
  for(ll i = 1; i <= n; ++i){
    deque<ll> q;
    for(ll j = 1; j < b; ++j){
      while(!q.empty() and h[i][j] < q.back()) q.pop_back();
      q.push_back(h[i][j]);
    }
    for(ll j = 1; j <= m-b+1; ++j){
      while(!q.empty() and h[i][j+b-1] < q.back()) q.pop_back();
      q.push_back(h[i][j+b-1]);
      hmin[i][j] = q.front();
      if(q.front() == h[i][j]) q.pop_front();
    }
  }
  ll ans = 0;
  for(ll j = 1; j <= m-b+1; ++j){
    deque<ll> q;
    for(ll i = 1; i < a; ++i){
      while(!q.empty() and hmin[i][j] < q.back()) q.pop_back();
      q.push_back(hmin[i][j]);
    }
    for(ll i = 1; i <= n-a+1; ++i){
      while(!q.empty() and hmin[i+a-1][j] < q.back()) q.pop_back();
      q.push_back(hmin[i+a-1][j]);
      ans += q.front();
      if(q.front() == hmin[i][j]) q.pop_front();
    }
  }
  cout << ans;
  return 0;
}
```

