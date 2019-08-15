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

