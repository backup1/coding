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

