# Dynamic Segment Tree

## Modify one element + Query on interval

2e5 insertions and 2e5 queries

```cpp
#include <bits/stdc++.h>
#include <ext/pb_ds/assoc_container.hpp>
using namespace __gnu_pbds;
using namespace std;
using ll = long long;
ll n;

const int RANDOM = chrono::high_resolution_clock::now().time_since_epoch().count();
struct chash{
    int operator()(int x) const { return x^RANDOM; }
};
gp_hash_table<ll, ll, chash> seg;

ll get(ll x){
  return (seg.find(x) == seg.end()) ? 0 : seg[x];
}

void build(){
  for(int i = n-1; i > 0; --i) seg[i] = get(i<<1) + get(i<<1|1);
}

void modify(ll p, ll val){
  for(seg[p += n] = val; p > 0; p >>= 1) seg[p>>1] = get(p) + get(p^1);
}

ll query(ll l, ll r){
  ll res = 0;
  for(l += n,r += n; l < r; l >>= 1,r >>= 1){
    if(l&1) res += get(l++);
    if(r&1) res += get(--r);
  }
  return res;
}

int main(){
  cin >> n;
  ll d;
  for(int i = 0; i < n; ++i){
    cin >> d;
    seg[n+i] = d;
  }
  build();
  modify(0, 1);
  printf("%d\n", query(3, 11));
  return 0;
}
```

