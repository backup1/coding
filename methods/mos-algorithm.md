# Mo's algorithm

{% embed url="https://www.hackerearth.com/fr/practice/notes/mos-algorithm/" %}

The algorithm is applicable if all following conditions are met:

1. Arr is not changed by queries;
2. All queries are known beforehand \(techniques requiring this property are often called “offline algorithms”\);
3. If we know Func\(\[L, R\]\), then we can compute Func\(\[L + 1, R\]\), Func\(\[L - 1, R\]\), Func\(\[L, R + 1\]\) and Func\(\[L, R - 1\]\), each in **O\(F\)** time.

Mo’s algorithm provides a way to **answer all queries in O\(\(N + Q\) \* sqrt\(N\) \* F\) time** with at least O\(Q\) additional memory.

{% embed url="https://www.spoj.com/problems/DQUERY/" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
const int A = 300005,N = 1000005,Q = 200005;
const int BLOCK = 555;
int a[A],m[N],cnt = 0;
tuple<int,int,int> queries[Q];
int ans[Q];
inline bool mo_cmp(const tuple<int,int,int>& x,const tuple<int,int,int>& y){
  int block_x = get<0>(x)/BLOCK;
  int block_y = get<0>(y)/BLOCK;
  if(block_x != block_y) return block_x < block_y;
  return get<1>(x) < get<1>(y);
}
inline void add(int val){
  ++m[val];
  if(m[val] == 1) ++cnt;
}
inline void remove(int val){
  --m[val];
  if(m[val] == 0) --cnt;
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int n,q,l,r,idx;
  scanf("%d",&n);
  for(int i = 0; i < n; ++i) scanf("%d",&a[i]);
  scanf("%d",&q);
  for(int i = 0; i < q; ++i){
    scanf("%d%d",&l,&r);
    queries[i] = {l-1,r-1,i};
  }
  sort(queries,queries+q,mo_cmp);
  int mo_left = 0,mo_right = -1;
  for(int i = 0; i < q; ++i){
    tie(l,r,idx) = queries[i];
    while(mo_right < r) ++mo_right, add(a[mo_right]);
    while(mo_right > r) remove(a[mo_right]), mo_right--;
    while(mo_left < l) remove(a[mo_left]), mo_left++;
    while(mo_left > l) --mo_left, add(a[mo_left]);
    ans[idx] = cnt;
  }
  for(int i = 0; i < q; ++i) printf("%d\n",ans[i]);
  return 0;
}
```

{% embed url="https://codeforces.com/problemset/problem/86/D" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = int64_t;
const ll N = 1e6+5;
const ll bloc = 500;
ll cnt[N],psum;
inline void add(int val){
  psum += (2*cnt[val]+1)*val;
  ++cnt[val];
}
inline void remove(int val){
  --cnt[val];
  psum -= (2*cnt[val]+1)*val;
}
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  ll n,t,l,r,idx;
  cin >> n >> t;
  vector<ll> a(n);
  for(ll i = 0; i < n; ++i) cin >> a[i];
  vector<tuple<ll,ll,ll>> cmds;
  for(ll i = 0; i < t; ++i){
    cin >> l >> r;
    cmds.emplace_back(l-1,r-1,i);
  }
  sort(begin(cmds),end(cmds),[](const tuple<ll,ll,ll>& c1,const tuple<ll,ll,ll>& c2){
    int b1 = get<0>(c1)/bloc, b2 = get<0>(c2)/bloc;
    if(b1 != b2) return b1 < b2;
    return get<1>(c1) < get<1>(c2);
  });
  vector<ll> ans(t);
  psum = 0;
  ll mo_left = 0,mo_right = -1;
  for(auto& tpl : cmds){
    tie(l,r,idx) = tpl;
    while(mo_right < r) ++mo_right, add(a[mo_right]);
    while(mo_right > r) remove(a[mo_right]), mo_right--;
    while(mo_left < l) remove(a[mo_left]), mo_left++;
    while(mo_left > l) --mo_left, add(a[mo_left]);
    ans[idx] = psum;
  }
  for(int i = 0; i < t; ++i) cout << ans[i] << '\n';
  return 0;
}
```

{% embed url="https://codeforces.com/problemset/problem/220/B" %}

best bloc size is not 100, by the way :-\)

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1e5;
const int bloc = 100;
vector<int> a;
int cnt[N+5],psum;
inline void add(int val){
  if(val <= N){
    ++cnt[val];
    if(cnt[val] == val) ++psum;
    else if(cnt[val] == val+1) --psum;
  }
}
inline void remove(int val){
  if(val <= N){
    --cnt[val];
    if(cnt[val] == val) ++psum;
    else if(cnt[val] == val-1) --psum;
  }
}
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  int n,m,l,r,idx;
  cin >> n >> m;
  a = vector<int>(n);
  for(int i = 0; i < n; ++i) cin >> a[i];
  vector<tuple<int,int,int>> queries(m);
  for(int i = 0; i < m; ++i){
    cin >> l >> r;
    queries[i] = make_tuple(l-1,r-1,i);
  }
  sort(begin(queries),end(queries),[](const tuple<int,int,int>& t1,const tuple<int,int,int>& t2){
    int b1 = get<0>(t1)/bloc, b2 = get<0>(t2)/bloc;
    if(b1 != b2) return b1 < b2;
    return get<1>(t1) < get<1>(t2);
  });
  vector<int> ans(m);
  psum = 0;
  int mo_left = 0,mo_right = -1;
  for(auto& t : queries){
    tie(l,r,idx) = t;
    while(mo_right < r) ++mo_right, add(a[mo_right]);
    while(mo_right > r) remove(a[mo_right]), mo_right--;
    while(mo_left < l) remove(a[mo_left]), mo_left++;
    while(mo_left > l) --mo_left, add(a[mo_left]);
    ans[idx] = psum;
  }
  for(int i : ans) cout << i << '\n';
  return 0;
}
```

{% embed url="https://codeforces.com/contest/617/problem/E" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = int64_t;
const ll N = (1<<20)+5;
const ll bloc = 333;
ll cnt[N],ret,k;
vector<ll> prefix;
inline void add(ll val){
  ret += cnt[val^k];
  ++cnt[val];
}
inline void remove(ll val){
  --cnt[val];
  ret -= cnt[val^k];
}
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  ll n,m,l,r,idx;
  cin >> n >> m >> k;
  prefix = vector<ll>(n+1);
  for(ll i = 1; i <= n; ++i){
    cin >> prefix[i];
    prefix[i] ^= prefix[i-1];
  }
  vector<tuple<ll,ll,ll>> queries(m);
  for(ll i = 0; i < m; ++i){
    cin >> l >> r;
    queries[i] = make_tuple(l,r,i);
  }
  sort(begin(queries),end(queries),[](const tuple<ll,ll,ll>& t1,const tuple<ll,ll,ll>& t2){
    ll b1 = get<0>(t1)/bloc, b2 = get<0>(t2)/bloc;
    if(b1 != b2) return b1 < b2;
    return get<1>(t1) < get<1>(t2);
  });
  vector<ll> ans(m);
  ret = 0;
  ll mo_left = 0,mo_right = -1;
  for(auto& t : queries){
    tie(l,r,idx) = t;
    --l;
    while(mo_right < r) ++mo_right, add(prefix[mo_right]);
    while(mo_right > r) remove(prefix[mo_right]), mo_right--;
    while(mo_left < l) remove(prefix[mo_left]), mo_left++;
    while(mo_left > l) --mo_left, add(prefix[mo_left]);
    ans[idx] = ret;
  }
  for(ll i : ans) cout << i << '\n';
  return 0;
}
```

