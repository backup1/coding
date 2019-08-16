# 2D Segment Tree

## With index compression

{% embed url="https://www.codechef.com/JCWR2019/problems/JCWC04" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
vector<vector<ll>> bit;
inline getSum2(ll x,ll y1,ll y2){
  ll ret = 0;
  while(y1 <= y2){
    if(y1&1){
      ret += bit[x][y1];
      ++y1;
    }
    if(y2%2 == 0){
      ret += bit[x][y2];
      --y2;
    }
    y1 >>= 1;
    y2 >>= 1;
  }
  return ret;
}
inline ll getSum(ll x1,ll x2,ll y1,ll y2){
  ll ret = 0;
  while(x1 <= x2){
    if(x1&1){
      ret += getSum2(x1,y1,y2);
      ++x1;
    }
    if(x2%2 == 0){
      ret += getSum2(x2,y1,y2);
      --x2;
    }
    x1 >>= 1;
    x2 >>= 1;
  }
  return ret;
}
inline void update(ll x,ll y,ll val){
  while(y > 0){
    bit[x][y] += val;
    y /= 2;
  }
  while(x > 1){
    x /= 2;
    bit[x][1] += val;
  }
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int T;
  cin >> T;
  while(T--){
    ll N,M,K,R,C,S;
    cin >> N >> M >> K;
    tuple<ll,ll,ll> vt[K];
    unordered_set<ll> Rst,Cst;
    for(ll i = 0; i < K; ++i){
      cin >> R >> C >> S;
      vt[i] = make_tuple(R,C,S);
      Rst.insert(R);
      Cst.insert(C);
    }
    vector<ll> Rs,Cs;
    Rs.assign(begin(Rst),end(Rst));
    Cs.assign(begin(Cst),end(Cst));
    sort(begin(Rs),end(Rs));
    sort(begin(Cs),end(Cs));
    unordered_map<ll,ll> Rid,Cid;
    for(ll i = 0; i < Rs.size(); ++i) Rid[Rs[i]] = i;
    for(ll i = 0; i < Cs.size(); ++i) Cid[Cs[i]] = i;
    ll n = Rs.size(), m = Cs.size();
    bit = vector<vector<ll>>(2*n,vector<ll>(2*m));
    for(auto& p : vt){
      tie(R,C,S) = p;
      ll r = Rid[R], c = Cid[C];
      update(n+r-1,m+c-1,S);
    }
    ll Q,X1,X2,Y1,Y2;
    cin >> Q;
    tuple<ll,ll,ll,ll> qs[Q];
    unordered_map<ll,ll> Xidx1,Xidx2,Yidx1,Yidx2;
    for(ll i = 0; i < Q; ++i){
      cin >> X1 >> Y1 >> X2 >> Y2;
      qs[i] = make_tuple(X1,Y1,X2,Y2);
      if(!Xidx1.count(X1)){
        Xidx1[X1] = lower_bound(begin(Rs),end(Rs),X1)-begin(Rs);
      }
      if(!Xidx2.count(X2)){
        Xidx2[X2] = upper_bound(begin(Rs),end(Rs),X2)-begin(Rs);
      }
      if(!Yidx1.count(Y1)){
        Yidx1[Y1] = lower_bound(begin(Cs),end(Cs),Y1)-begin(Cs);
      }
      if(!Yidx2.count(Y2)){
        Yidx2[Y2] = upper_bound(begin(Cs),end(Cs),Y2)-begin(Cs);
      }
    }
    for(auto& p : qs){
      tie(X1,Y1,X2,Y2) = p;
      ll x1 = n+Xidx1[X1], x2 = n+Xidx2[X2]-1;
      ll y1 = m+Yidx1[Y1], y2 = m+Yidx2[Y2]-1;
      cout << getSum(x1,x2,y1,y2) << '\n';
    }
  }
  return 0;
}
```

