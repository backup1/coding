# Lazy Segment Tree

{% embed url="https://codeforces.com/contest/242/problem/E" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
vector<vector<ll>> segt,lazy;
inline ll getSum(ll n,ll idx,ll left,ll right,ll L,ll R){
  if(lazy[n][idx] == 1){
    segt[n][idx] = right-left-segt[n][idx];
    if(right-left > 1){
      lazy[n][2*idx] ^= 1;
      lazy[n][2*idx+1] ^= 1;
    }
    lazy[n][idx] = 0;
  }
  if(L >= right or R <= left) return 0ll;
  if((right-left <= 1) or (left == L and right == R)){
    return segt[n][idx];
  }
  ll mid = (left+right)/2;
  ll ans = 0;
  ans += getSum(n,2*idx,left,mid,L,min(R,mid));
  ans += getSum(n,2*idx+1,mid,right,max(L,mid),R);
  segt[n][idx] = segt[n][2*idx] + segt[n][2*idx+1];
  return ans;
}
inline void updateLazy(ll n,ll idx,ll left,ll right,ll L,ll R){
  if(lazy[n][idx] == 1){
    segt[n][idx] = right-left-segt[n][idx];
    if(right-left > 1){
      lazy[n][2*idx] ^= 1;
      lazy[n][2*idx+1] ^= 1;
    }
    lazy[n][idx] = 0;
  }
  if(L >= right or R <= left) return;
  if((right-left <= 1) or (L == left and R == right)){
    segt[n][idx] = right-left-segt[n][idx];
    if(right-left > 1){
      lazy[n][2*idx] ^= 1;
      lazy[n][2*idx+1] ^= 1;
    }
    return;
  }
  if(L >= right or R <= left) return;
  ll mid = (left+right)/2;
  updateLazy(n,2*idx,left,mid,L,min(R,mid));
  updateLazy(n,2*idx+1,mid,right,max(L,mid),R);
  segt[n][idx] = segt[n][2*idx] + segt[n][2*idx+1];
}
inline void update(ll n,ll idx,ll left,ll right,ll pos,ll val){
  if(val == 0ll) return;
  if(right-left <= 1){
    segt[n][idx] = val;
    return;
  }
  ll mid = (left+right)/2;
  if(pos < mid) update(n,2*idx,left,mid,pos,val);
  else update(n,2*idx+1,mid,right,pos,val);
  segt[n][idx] = segt[n][2*idx]+segt[n][2*idx+1];
}
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  ll n,m,a,t,l,r,x;
  cin >> n;
  segt = vector<vector<ll>>(20,vector<ll>(4*n+5));
  lazy = vector<vector<ll>>(20,vector<ll>(4*n+5));
  for(ll i = 1; i <= n; ++i){
    cin >> a;
    for(ll j = 0; j < 20; ++j){
      if(a&(1<<j)) update(j,1,1,n+1,i,1);
    }
  }
  cin >> m;
  while(m--){
    cin >> t >> l >> r;
    if(t == 1){
      ll ans = 0;
      for(ll j = 0; j < 20; ++j) ans += (1<<j)*getSum(j,1,1,n+1,l,r+1);
      cout << ans << '\n';
    }
    else{
      cin >> x;
      for(ll j = 0; j < 20; ++j){
        if(x&(1<<j)) updateLazy(j,1,1,n+1,l,r+1);
      }
    }
  }
  return 0;
}
```

{% embed url="https://www.spoj.com/problems/HORRIBLE/" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
vector<ll> segt,lazy;
ll getSum(ll idx,ll left,ll right,ll L,ll R){
  if(lazy[idx]){
    segt[idx] += (right-left)*lazy[idx];
    if(right-left > 1){
      lazy[2*idx] += lazy[idx];
      lazy[2*idx+1] += lazy[idx];
    }
    lazy[idx] = 0;
  }
  if(right-left == 1 or (L == left and R == right)){
    return segt[idx];
  }
  ll mid = (left+right)/2,ans = 0;
  if(L < mid) ans += getSum(2*idx,left,mid,L,min(mid,R));
  if(R > mid) ans += getSum(2*idx+1,mid,right,max(L,mid),R);
  return ans;
}
void update(ll idx,ll left,ll right,ll L,ll R,ll val){
  if(right-left == 1 or (L == left and R == right)){
    lazy[idx] += val;
    return;
  }
  ll mid = (left+right)/2;
  if(L < mid) update(2*idx,left,mid,L,min(mid,R),val);
  if(R > mid) update(2*idx+1,mid,right,max(L,mid),R,val);
  segt[idx] += (R-L)*val;
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  ll T;
  cin >> T;
  while(T--){
    int cmd;
    ll N,C,p,q,val;
    cin >> N >> C;
    segt = vector<ll>(4*N+5);
    lazy = vector<ll>(4*N+5);
    while(C--){
      cin >> cmd;
      if(cmd == 0){
        cin >> p >> q >> val;
        update(1,1,N+1,p,q+1,val);
      }
      else {
        cin >> p >> q;
        cout << getSum(1,1,N+1,p,q+1) << '\n';
      }
    }
  }
  return 0;
}
```

{% embed url="https://www.spoj.com/problems/ZING01/" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 100005;
int n;
vector<vector<int>> segt(26,vector<int>(4*N+5)),lazy(26,vector<int>(4*N+5,-1));
inline void update(int n,int idx,int left,int right,int pos,int val){
  if(right-left <= 1){
    segt[n][idx] = val;
    return;
  }
  int mid = (left+right)/2;
  if(pos < mid) update(n,2*idx,left,mid,pos,val);
  else update(n,2*idx+1,mid,right,pos,val);
  segt[n][idx] = segt[n][2*idx] + segt[n][2*idx+1];
}
inline void updateLazy(int n,int idx,int left,int right,int L,int R,int val){
  if(lazy[n][idx] != -1){
    segt[n][idx] = (right-left)*lazy[n][idx];
    lazy[n][2*idx] = lazy[n][idx];
    lazy[n][2*idx+1] = lazy[n][idx];
    lazy[n][idx] = -1;
  }
  if(L >= right or R <= left) return;
  if((right-left == 1) or (L == left and R == right)){
    segt[n][idx] = (right-left)*val;
    if(right-left > 1){
      lazy[n][2*idx] = val;
      lazy[n][2*idx+1] = val;
    }
    lazy[n][idx] = -1;
    return;
  }
  int mid = (left+right)/2;
  updateLazy(n,2*idx,left,mid,L,min(R,mid),val);
  updateLazy(n,2*idx+1,mid,right,max(L,mid),R,val);
  segt[n][idx] = segt[n][2*idx] + segt[n][2*idx+1];
}
inline int getSum(int n,int idx,int left,int right,int L,int R){
  if(lazy[n][idx] != -1){
    segt[n][idx] = (right-left)*lazy[n][idx];
    if(right-left > 1){
      lazy[n][2*idx] = lazy[n][idx];
      lazy[n][2*idx+1] = lazy[n][idx];
    }
    lazy[n][idx] = -1;
  }
  if(L >= right or R <= left) return 0;
  if((right-left == 1) or (left == L and right == R)){
    return segt[n][idx];
  }
  int mid = (left+right)/2;
  int ans = 0;
  ans += getSum(n,2*idx,left,mid,L,min(R,mid));
  ans += getSum(n,2*idx+1,mid,right,max(L,mid),R);
  segt[n][idx] = segt[n][2*idx] + segt[n][2*idx+1];
  return ans;
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  string s;
  cin >> s;
  n = s.size();
  for(int i = 0; i < n; ++i) update(s[i]-'a',1,0,n,i,1);
  int q,cmd,l,r,k;
  char c;
  cin >> q;
  while(q--){
    cin >> cmd;
    if(cmd == 0){
      cin >> l >> r >> c;
      --l;
      for(int i = 0; i < 26; ++i){
        if(i == c-'a') updateLazy(i,1,0,n,l,r,1);
        else updateLazy(i,1,0,n,l,r,0);
      }
    }
    else{
      cin >> k >> c;
      int tot = getSum(c-'a',1,0,n,0,n);
      if(tot < k) cout << "-1\n";
      else{
        int b = 0, e = n, k2 = k;
        while(b+1 < e){
          int mid = (b+e)/2;
          int tmp = getSum(c-'a',1,0,n,b,mid);
          if(tmp >= k2) e = mid;
          else{
            k2 -= tmp;
            b = mid;
          }
        }
        int tmp = getSum(c-'a',1,0,n,0,b+1);
        if(tmp >= k){
          if(k == getSum(c-'a',1,0,n,0,b)) cout << b << '\n';
          else cout << b+1 << '\n';
        }
        else{
          if(k == getSum(c-'a',1,0,n,0,b+2)) cout << b+2 << '\n';
          else cout << b+3 << '\n';
        }
      }
    }
  }
  return 0;
}
```

2008/Special/Gold/holpaint

```cpp
#include <bits/stdc++.h>
using namespace std;
vector<int> sample[15],segt[15],lazy[15];
void init_sample(int slot,int idx,int left,int right,int L,int R,vector<int>& val){
  if(R < L) return;
  if(left == right){
    sample[slot][idx] = val[left];
    return;
  }
  int mid = (left+right)/2;
  init_sample(slot,2*idx,left,mid,L,min(R,mid),val);
  init_sample(slot,2*idx+1,mid+1,right,max(L,mid+1),R,val);
  sample[slot][idx] = sample[slot][2*idx] + sample[slot][2*idx+1];
}
void init_segt(int slot,int idx,int left,int right,int L,int R){
  if(R < L) return;
  if(left == right){
    if(sample[slot][idx] == 0) segt[slot][idx] = 1;
    return;
  }
  int mid = (left+right)/2;
  init_segt(slot,2*idx,left,mid,L,min(R,mid));
  init_segt(slot,2*idx+1,mid+1,right,max(L,mid+1),R);
  segt[slot][idx] = segt[slot][2*idx] + segt[slot][2*idx+1];
}
void update(int slot,int idx,int left,int right,int L,int R,int val){
  if(R < L) return;
  if((left == L and right == R) or left == right){
    lazy[slot][idx] = val;
    if(val) segt[slot][idx] = sample[slot][idx];
    else segt[slot][idx] = right-left+1-sample[slot][idx];
    return;
  }
  int mid = (left+right)/2;
  if(lazy[slot][idx] == 0){
    lazy[slot][2*idx] = 0;
    lazy[slot][2*idx+1] = 0;
    segt[slot][2*idx] = mid-left+1-sample[slot][2*idx];
    segt[slot][2*idx+1] = right-mid-sample[slot][2*idx+1];
  }
  else if(lazy[slot][idx] == 1){
    lazy[slot][2*idx] = 1;
    lazy[slot][2*idx+1] = 1;
    segt[slot][2*idx] = sample[slot][2*idx];
    segt[slot][2*idx+1] = sample[slot][2*idx+1];
  }
  lazy[slot][idx] = -1;
  update(slot,2*idx,left,mid,L,min(R,mid),val);
  update(slot,2*idx+1,mid+1,right,max(L,mid+1),R,val);
  segt[slot][idx] = segt[slot][2*idx] + segt[slot][2*idx+1];
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int r,c,q,r1,r2,c1,c2,x;
  cin >> r >> c >> q;
  vector<string> vs(r);
  for(int i = 0; i < r; ++i) cin >> vs[i];
  vector<int> val(r);
  for(int slot = 0; slot < c; ++slot){
    sample[slot].resize(4*r+5);
    segt[slot].resize(4*r+5);
    lazy[slot].resize(4*r+5);
    fill(begin(lazy[slot]),end(lazy[slot]),-1);
    for(int j = 0; j < r; ++j){
      if(vs[j][slot] == '1') val[j] = 1;
      else val[j] = 0;
    }
    init_sample(slot,1,0,r-1,0,r-1,val);
    init_segt(slot,1,0,r-1,0,r-1);
  }
  for(int i = 0; i < q; ++i){
    cin >> r1 >> r2 >> c1 >> c2 >> x;
    --r1, --r2, --c1, --c2;
    int ans = 0;
    for(int j = 0; j < c; ++j){
      if(c1 <= j and j <= c2) update(j,1,0,r-1,r1,r2,x);
      ans += segt[j][1];
    }
    cout << ans << '\n';
  }
  return 0;
}
```

[https://codeforces.com/problemset/problem/52/C](https://codeforces.com/problemset/problem/52/C)

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const ll inf = LLONG_MAX/2;
struct node {
  ll val,lz;
};
vector<node> segt;
void init(ll idx,ll l,ll r,ll pos,ll val){
  if(pos < l or r < pos) return;
  if(l == pos and r == pos){
    segt[idx].val = val;
    segt[idx].lz = 0;
    return;
  }
  ll mid = (l+r)/2;
  if(pos <= mid) init(2*idx,l,mid,pos,val);
  else init(2*idx+1,mid+1,r,pos,val);
  segt[idx].val = min(segt[2*idx].val,segt[2*idx+1].val);
}
void update(ll idx,ll l,ll r,ll L,ll R,ll delta){
  if(R < l or r < L or R < L) return;
  if(L <= l and r <= R){
    segt[idx].val += delta;
    segt[idx].lz += delta;
    return;
  }
  if(segt[idx].lz){
    segt[2*idx].val += segt[idx].lz;
    segt[2*idx].lz += segt[idx].lz;
    segt[2*idx+1].val += segt[idx].lz;
    segt[2*idx+1].lz += segt[idx].lz;
    segt[idx].lz = 0;
  }
  ll mid = (l+r)/2;
  update(2*idx,l,mid,L,R,delta);
  update(2*idx+1,mid+1,r,L,R,delta);
  segt[idx].val = min(segt[2*idx].val,segt[2*idx+1].val);
}
ll query(ll idx,ll l,ll r,ll L,ll R){
  if(R < l or r < L or R < L) return inf;
  if(L <= l and r <= R) return segt[idx].val;
  if(segt[idx].lz){
    segt[2*idx].val += segt[idx].lz;
    segt[2*idx].lz += segt[idx].lz;
    segt[2*idx+1].val += segt[idx].lz;
    segt[2*idx+1].lz += segt[idx].lz;
    segt[idx].lz = 0;
  }
  ll mid = (l+r)/2;
  ll ans = query(2*idx,l,mid,L,R);
  ans = min(ans,query(2*idx+1,mid+1,r,L,R));
  return ans;
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  ll n,a,m,l,r;
  scanf("%lld",&n);
  segt.resize(4*n+5);
  for(ll i = 0; i < n; ++i){
    scanf("%lld",&a);
    init(1,0,n-1,i,a);
  }
  scanf("%lld",&m);
  while(m--){
    scanf("%lld%lld",&l,&r);
    if(getchar() == ' '){
      scanf("%lld",&a);
      if(l <= r) update(1,0,n-1,l,r,a);
      else{
        update(1,0,n-1,0,r,a);
        update(1,0,n-1,l,n-1,a);
      }
    }
    else{
      ll ans = INT_MAX;
      if(l <= r) ans = query(1,0,n-1,l,r);
      else{
        ans = query(1,0,n-1,0,r);
        ans = min(ans,query(1,0,n-1,l,n-1));
      }
      cout << ans << '\n';
    }
  }
  return 0;
}
```

