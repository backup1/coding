# Combinatorial and Factorial

## Factorial and its modular reverse

```cpp
using ll = int64_t;
const ll SZ = 2e5+5;
inline ll Pow(ll a,ll p){
  ll ret = 1,cp = a;
  while(p){
    if(p&1) ret = (ret*cp)%mod;
    p >>= 1;
    cp = (cp*cp)%mod;
  }
  return ret;
}
inline ll Inverse(ll a){
  return Pow(a,mod-2);
}
vector<ll> fact(SZ,1),factinv(SZ,1);
inline void init(){
  for(ll i = 2; i < SZ; ++i){
    fact[i] = (fact[i-1]*i)%mod;
  }
  factinv[SZ-1] = Inverse(fact[SZ-1]);
  for(ll i = SZ-2; i > 1; --i){
    factinv[i] = (factinv[i+1]*(i+1))%mod;
  }
}
```

## Combinatorial

$$
C_n^a = \frac{n!}{a! \times (n-a)!} = C_n^{n-a}
$$

```cpp
using ll = int64_t;
const ll SZ = 2e5+5;
inline ll Pow(ll a,ll p){
  ll ret = 1,cp = a;
  while(p){
    if(p&1) ret = (ret*cp)%mod;
    p >>= 1;
    cp = (cp*cp)%mod;
  }
  return ret;
}
inline ll Inverse(ll a){
  return Pow(a,mod-2);
}
vector<ll> fact(SZ,1),factinv(SZ,1);
inline void init(){
  for(ll i = 2; i < SZ; ++i){
    fact[i] = (fact[i-1]*i)%mod;
  }
  factinv[SZ-1] = Inverse(fact[SZ-1]);
  for(ll i = SZ-2; i > 1; --i){
    factinv[i] = (factinv[i+1]*(i+1))%mod;
  }
}
inline ll C(ll n,ll a){
  if(a < 0 or a > n) return 0;
  ll ans = fact[n];
  ans = (ans*factinv[a])%mod;
  ans = (ans*factinv[n-a])%mod;
  return ans;
}
```

[https://atcoder.jp/contests/arc068/submissions/9201778](https://atcoder.jp/contests/arc068/submissions/9201778)

```cpp
#include<bits/stdc++.h>
using namespace std;

using ll = long long;

const int MOD = int(1e9)+7;

int modinv(int a, int m) {
  return (a == 1) ? 1 : m - int(ll(modinv(m % a, a)) * m / a);
}

int fact(int n) {
  int res = 1;
  for (int i = 1; i <= n; i++) res = int(ll(res) * i % MOD);
  return res;
}

int choose(int n, int r) {
  if (r<0 || r>n) return 0;
  return ll(fact(n)) * modinv(ll(fact(r)) * ll(fact(n-r)) % MOD, MOD) % MOD;
}

int solve(int N, int K) {
  int ans = choose(N-1+K-1, K-1) - choose(N-1+K-1, K-2);
  if (ans < 0) ans += MOD;
  for (int i = K+1; i <= N-1; i++) {
    ans *= 2;
    if (ans >= MOD) ans -= MOD;
  }
  return ans;
}

int main() {
  int N, K; cin >> N >> K;
  cout << solve(N, K) << '\n';
}
```



