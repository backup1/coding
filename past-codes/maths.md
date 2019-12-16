# Maths

## Modular Power

[https://atcoder.jp/contests/m-solutions2019/tasks/m\_solutions2019\_e](https://atcoder.jp/contests/m-solutions2019/tasks/m_solutions2019_e)

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const ll MOD = 1e6+3; // prime number
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
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  vector<ll> inv(MOD,1),fact(MOD,1);
  for(ll i = 2; i < MOD; ++i){
    inv[i] = getInverse(i);
    fact[i] = (i*fact[i-1])%MOD;
  }
  ll q,x,d,n;
  cin >> q;
  while(q--){
    cin >> x >> d >> n;
    if(d == 0) cout << getPow(x,n);
    else if(x == 0) cout << 0;
    else{
      ll X = (x*inv[d])%MOD;
      if(X+n-1 >= MOD) cout << 0;
      else{
        ll ret = (fact[X+n-1]*inv[fact[X-1]])%MOD;
        cout << (ret*getPow(d,n))%MOD;
      }
    }
    cout << '\n';
  }
  return 0;
}
```

## Binomial Coefficients

[https://atcoder.jp/contests/abc042/tasks/arc058\_b](https://atcoder.jp/contests/abc042/tasks/arc058_b)

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

