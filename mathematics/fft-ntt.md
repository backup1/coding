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

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = int64_t;
const ll mod = 998244353, G=3;
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
  ll rt[1<<20];
  void fi(int n) {
    rt[n/2] = 1;
    rt[n/2+1] = Pow(G,(mod-1)/n);
    for(int i = n/2+2; i < n; ++i) rt[i] = rt[i-1]*rt[n/2+1]%mod;
    for(int i = n/2; --i; ) rt[i] = rt[2*i];
  }
  void ac(vector<ll> &a){
    int n = a.size();
    for(int i = 0, j = 0; i < n; ++i){
      if(i > j) swap(a[i],a[j]);
      int k = n/2;
      while((j ^= k) < k) k >>= 1;
    }
    for(int st = 1; (st<<1) <= n; st <<= 1){
      for(int i = 0; i < n; i += (st<<1)){
        for(int j = i; j < i+st; ++j){
          ll z = rt[j-i+st]*a[j+st]%mod;
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
ll iv[(1<<20)+1];
inline vector<ll> mult(vector<ll>& v, vector<ll>& w){
  int s = v.size()+w.size()-1, t = 1;
  while(t < s) t <<= 1;
  v.resize(t);
  w.resize(t);
  nt.ac(v);
  nt.ac(w);
  for(int i = 0; i < t; ++i) v[i] = v[i]*w[i]%mod *iv[t]%mod;
  reverse(v.begin()+1,v.end());
  nt.ac(v);
  while(v.back() == 0) v.pop_back();
  return v;
}
inline vector<ll> trans(vector<ll>& v,ll t){
  vector<ll> a(v.size()),b(v.size()),r(v.size());
  ll f = 1, fi = 1, tp = 1;
  for(int i = 0; i < v.size(); ++i){
    if(i){
      f = f*i%mod;
      fi = fi*iv[i]%mod;
      tp = tp*t%mod;
    }
    a[v.size()-1-i] = f*v[i]%mod;
    b[i] = tp*fi%mod;
  }
  vector<ll> c = mult(a,b);
  fi = 1;
  for(int i = 0; i < v.size(); ++i){
    if(i) fi = fi*iv[i]%mod;
    r[i] = c[v.size()-1-i]*fi%mod;
  }
  return r;
}
inline vector<ll> fp(int r){
  if(r == 0) return {1};
  vector<ll> v = fp(r/2),w = trans(v,mod-r/2);
  v = mult(v,w);
  if(r&1){
    v.push_back(0);
    for(int i = v.size()-2; ~i; --i){
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
  for(int i = 0; i <= r; ++i){
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
  nt.fi(1<<20);
  iv[1] = 1;
  for(int i = 2; i <= (1<<20); ++i) iv[i] = mod-mod/i*iv[mod%i]%mod;
  int t;
  cin >> t;
  while(t--) solve();
}
```

