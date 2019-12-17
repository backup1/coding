# FFT / NTT

## Primitive Root \(Modular Prime Number\)

[https://cp-algorithms.com/algebra/primitive-root.html](https://cp-algorithms.com/algebra/primitive-root.html)

```cpp
using ll = int64_t;
const ll mod = 1e9+7;
inline ll Pow(ll a,ll p){
  ll ret = 1,cp = a;
  while(p){
    if(p&1) ret = (ret*cp)%mod;
    p >>= 1;
    cp = (cp*cp)%mod;
  }
  return ret;
}
inline ll PrimitiveRoot(){
  vector<ll> fact;
  ll phi = mod-1, n = phi;
  for(ll i = 2; i*i <= n; ++i){
    if(n%i == 0){
      fact.push_back (i);
      while(n%i == 0) n /= i;
    }
  }
  if(n > 1) fact.push_back(n);
  for(ll res = 2; res <= mod; ++res){
    bool ok = true;
    for(ll i = 0; i < fact.size() and ok; ++i){
      ok &= (Pow(res,phi/fact[i]) != 1);
    }
    if(ok) return res;
  }
  return -1;
}
```

## CodeChef - Binomial Fever

[https://www.codechef.com/DEC19A/problems/BINOFEV](https://www.codechef.com/DEC19A/problems/BINOFEV)

[https://www.youtube.com/watch?v=GIOGuDxWDLM](https://www.youtube.com/watch?v=GIOGuDxWDLM)

[https://www.codechef.com/viewsolution/28370219](https://www.codechef.com/viewsolution/28370219)

{% tabs %}
{% tab title="C++" %}
```cpp
#pragma GCC optimize("O3")
#pragma GCC target("sse4")
#include <bits/stdc++.h>
using namespace std;
using ll = int64_t;
const ll mod = 998244353, G = 3;
// 998244353 = 119 x 2^23, 3 is its one primitive root
inline ll Pow(ll b,int p){
  ll r = 1;
  while(p){
    if(p&1) r = r*b%mod;
    b = b*b%mod;
    p >>= 1;
  }
  return r;
}
struct ntt{
  ll root[1<<20];
  // fill root so that
  // ==> root[2^i] to root[2^(i+1)-1] are the 2^i-th (modular) roots
  void fill(int n){ // n is a power of 2, n = 2^m with m <= 20
    int halfn = (n>>1);
    root[halfn] = 1;
    // mod-1 is a multiple of 2^23
    // ==> so mod-1 is always a multiple of n
    root[halfn+1] = Pow(G,(mod-1)/n);
    for(register int i = halfn+2; i < n; ++i){
      root[i] = root[i-1]*root[halfn+1]%mod;
    }
    for(register int i = halfn; --i; ){
      root[i] = root[i<<1];
    }
  }
  void ac(vector<ll> &a){
    int n = a.size(); // n is a power of 2
    int halfn = (n>>1);
    // 0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 ==>
    // 0 8 4 12 2 10 6 14 1 9 5 13 3 11 7 15
    for(register int i = 0, j = 0; i < n; ++i){
      if(i > j) swap(a[i],a[j]);
      int k = halfn;
      while((j ^= k) < k) k >>= 1;
    }
    // evaluate polynome a on the n roots w of 1, i.e. w^n = 1 %mod
    for(register int st = 1; (st<<1) <= n; st <<= 1){
      for(register int i = 0; i < n; i += (st<<1)){
        for(register int j = i; j < i+st; ++j){
          ll z = root[j-i+st]*a[j+st]%mod;
          a[j+st] = a[j]-z;
          if(a[j+st] < 0) a[j+st] += mod;
          a[j] += z;
          if(a[j] >= mod) a[j] -= mod;
        }
      }
    }
  }
};
ntt nt;
ll inverse[(1<<20)+1];
inline vector<ll> mult(vector<ll>& v, vector<ll>& w){
  int s = v.size()+w.size()-1, t = 1;
  while(t < s) t <<= 1;
  v.resize(t);
  w.resize(t);
  nt.ac(v); // evaluate v from polynome
  nt.ac(w); // evaluate w from polynome
  for(register int i = 0; i < t; ++i){
    v[i] = v[i]*w[i]%mod *inverse[t]%mod;
  } // v is storing the evaluation of v*w
  reverse(v.begin()+1,v.end()); // tricky !
  // such reverse gives us the evaluation on the w^-1
  nt.ac(v); // get polynome from evaluation
  while(v.back() == 0) v.pop_back(); // moving leading 0
  return v;
}
// given f(x) via v, then evaluate f(x+t)
// useful for first kind of Stirling numbers evaluations
inline vector<ll> trans(vector<ll>& v,ll t){
  int lv = v.size();
  vector<ll> a(lv),b(lv),r(lv);
  ll f = 1, fi = 1, tp = 1;
  for(register int i = 0; i < lv; ++i){
    if(i){
      f = f*i%mod;
      fi = fi*inverse[i]%mod;
      tp = tp*t%mod;
    }
    a[lv-1-i] = f*v[i]%mod;
    b[i] = tp*fi%mod;
  }
  vector<ll> c = mult(a,b);
  fi = 1;
  for(register int i = 0; i < lv; ++i){
    if(i) fi = fi*inverse[i]%mod;
    r[i] = c[lv-1-i]*fi%mod;
  }
  return r;
}
inline vector<ll> fp(int r){
  if(r == 0) return {1};
  vector<ll> v = fp(r/2),w = trans(v,mod-r/2);
  v = mult(v,w); // <== f(x) * f(x-r/2)
  if(r&1){
    v.push_back(0);
    for(register int i = v.size()-2; ~i; --i){
      v[i+1] += v[i];
      if(v[i+1] >= mod) v[i+1] -= mod;
      v[i] = v[i]*(mod-r+1)%mod;
    }
  }
  return v;
}
inline void solve() {
  int n, p, r;
  cin >> n >> p >> r;
  vector<ll> v = fp(r);
  ll nu = Pow(p, n+1),ans = 0,nup = 1,pp = 1,inf = 1;
  for(register int i = 0; i <= r; ++i){
    ll x = (pp == 1) ? (n+1) : (nup-1)*Pow(pp-1,mod-2)%mod;
    ans += x*v[i]%mod;
    nup = nup*nu%mod;
    pp = pp*p%mod;
    if(i) inf = inf*i%mod;
  }
  inf = Pow(inf,mod-2);
  cout << ans%mod*inf%mod << "\n";
}
int main() {
  ios::sync_with_stdio(0);
  cin.tie(0);
  nt.fill(1<<20);
  inverse[1] = 1;
  for(register int i = 2; i <= (1<<20); ++i){
    inverse[i] = mod-mod/i*inverse[mod%i]%mod;
  }
  int t;
  cin >> t;
  while(t--) solve();
}
```
{% endtab %}

{% tab title="Cleaned version" %}
```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = int64_t;
const ll mod = 998244353, G = 3;
inline ll Pow(ll b,int p){
  ll r = 1;
  while(p){
    if(p&1) r = r*b%mod;
    b = b*b%mod;
    p >>= 1;
  }
  return r;
}
struct ntt{
  ll root[1<<20];
  void fill(int n){
    int halfn = (n>>1);
    root[halfn] = 1;
    root[halfn+1] = Pow(G,(mod-1)/n);
    for(register int i = halfn+2; i < n; ++i){
      root[i] = root[i-1]*root[halfn+1]%mod;
    }
    for(register int i = halfn; --i; ){
      root[i] = root[i<<1];
    }
  }
  void ac(vector<ll> &a){
    int n = a.size();
    int halfn = (n>>1);
    for(register int i = 0, j = 0; i < n; ++i){
      if(i > j) swap(a[i],a[j]);
      int k = halfn;
      while((j ^= k) < k) k >>= 1;
    }
    for(register int st = 1; (st<<1) <= n; st <<= 1){
      for(register int i = 0; i < n; i += (st<<1)){
        for(register int j = i; j < i+st; ++j){
          ll z = root[j-i+st]*a[j+st]%mod;
          a[j+st] = a[j]-z;
          if(a[j+st] < 0) a[j+st] += mod;
          a[j] += z;
          if(a[j] >= mod) a[j] -= mod;
        }
      }
    }
  }
};
ntt nt;
ll inverse[(1<<20)+1];
inline vector<ll> mult(vector<ll>& v, vector<ll>& w){
  int s = v.size()+w.size()-1, t = 1;
  while(t < s) t <<= 1;
  v.resize(t);
  w.resize(t);
  nt.ac(v);
  nt.ac(w);
  for(register int i = 0; i < t; ++i){
    v[i] = v[i]*w[i]%mod *inverse[t]%mod;
  }
  reverse(v.begin()+1,v.end());
  nt.ac(v);
  while(v.back() == 0) v.pop_back();
  return v;
}
inline vector<ll> trans(vector<ll>& v,ll t){
  int lv = v.size();
  vector<ll> a(lv),b(lv),r(lv);
  ll f = 1, fi = 1, tp = 1;
  for(register int i = 0; i < lv; ++i){
    if(i){
      f = f*i%mod;
      fi = fi*inverse[i]%mod;
      tp = tp*t%mod;
    }
    a[lv-1-i] = f*v[i]%mod;
    b[i] = tp*fi%mod;
  }
  vector<ll> c = mult(a,b);
  fi = 1;
  for(register int i = 0; i < lv; ++i){
    if(i) fi = fi*inverse[i]%mod;
    r[i] = c[lv-1-i]*fi%mod;
  }
  return r;
}
inline vector<ll> fp(int r){
  if(r == 0) return {1};
  vector<ll> v = fp(r/2),w = trans(v,mod-r/2);
  v = mult(v,w);
  if(r&1){
    v.push_back(0);
    for(register int i = v.size()-2; ~i; --i){
      v[i+1] += v[i];
      if(v[i+1] >= mod) v[i+1] -= mod;
      v[i] = v[i]*(mod-r+1)%mod;
    }
  }
  return v;
}
inline void solve() {
  int n, p, r;
  cin >> n >> p >> r;
  vector<ll> v = fp(r);
  ll nu = Pow(p, n+1),ans = 0,nup = 1,pp = 1,inf = 1;
  for(register int i = 0; i <= r; ++i){
    ll x = (pp == 1) ? (n+1) : (nup-1)*Pow(pp-1,mod-2)%mod;
    ans += x*v[i]%mod;
    nup = nup*nu%mod;
    pp = pp*p%mod;
    if(i) inf = inf*i%mod;
  }
  inf = Pow(inf,mod-2);
  cout << ans%mod*inf%mod << "\n";
}
int main() {
  ios::sync_with_stdio(0);
  cin.tie(0);
  nt.fill(1<<20);
  inverse[1] = 1;
  for(register int i = 2; i <= (1<<20); ++i){
    inverse[i] = mod-mod/i*inverse[mod%i]%mod;
  }
  int t;
  cin >> t;
  while(t--) solve();
}
```
{% endtab %}
{% endtabs %}

