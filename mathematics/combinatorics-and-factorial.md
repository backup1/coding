# Combinatorics and Factorial

## Factorial and its modular reverse

```cpp
inline ll getPow(ll a,ll p){
  ll ret = 1,cp = a;
  while(p){
    if(p&1) ret = (ret*cp)%MOD;
    p >>= 1;
    cp = (cp*cp)%MOD;
  }
  return ret;
}
inline ll getInverse(ll a){
  return getPow(a,MOD-2)%MOD;
}
vector<ll> fact(200001,1),factinv(200001,1);
inline void init(ll l){
  for(ll i = 1; i < l; ++i){
    fact[i] = (fact[i-1]*i)%MOD;
    factinv[i] = (factinv[i-1]*getInverse(i))%MOD;
  }
}
```

## Combinatorics

$$
C_{a+b}^a = \frac{(a+b)!}{a! \times b!} = C_{a+b}^b
$$

```cpp
ll C(ll a,ll b){
  ll ans = fact[a+b];
  ans = (ans*factinv[a])%MOD;
  ans = (ans*factinv[b])%MOD;
  return ans;
}
```

## Questions

{% embed url="https://atcoder.jp/contests/abc042/tasks/arc058\_b" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const ll MOD = 1e9+7;
ll getPow(ll a,ll p){
  ll ret = 1,cp = a;
  while(p){
    if(p&1) ret = (ret*cp)%MOD;
    p >>= 1;
    cp = (cp*cp)%MOD;
  }
  return ret;
}
ll getInverse(ll a){
  return getPow(a,MOD-2)%MOD;
}
vector<ll> fact(200001,1),factinv(200001,1);
void init(ll l){
  for(ll i = 1; i < l; ++i){
    fact[i] = (fact[i-1]*i)%MOD;
    factinv[i] = (factinv[i-1]*getInverse(i))%MOD;
  }
}
ll C(ll a,ll b){
  ll ans = fact[a+b];
  ans = (ans*factinv[a])%MOD;
  ans = (ans*factinv[b])%MOD;
  return ans;
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  ll h,w,a,b;
  cin >> h >> w >> a >> b;
  init(h+w);
  ll ans = 0;
  for(int j = b; j < w; ++j){
    ans = (ans+C(h-a-1,j)*C(a-1,w-j-1))%MOD;
  }
  cout << ans;
  return 0;
}
```

