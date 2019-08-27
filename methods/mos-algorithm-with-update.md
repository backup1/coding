# Mo's algorithm with update

We need to work with bloc size of $$n^{\frac{2}{3}}$$, so that all queries will be split into $$n^{\frac{2}{3}}$$groups. The complexity is $$O(n^{\frac{5}{3}})$$

The comparison function used to sort all queries is the following

```cpp
inline bool mo_cmp(const query& q1,const query& q2){
  int b1 = q1.left/BLOCK, b2 = q2.left/BLOCK;
  if(b1 != b2) return b1 < b2;
  int c1 = q1.right/BLOCK, c2 = q2.right/BLOCK;
  if(c1 != c2) return (b1&1) ? (c1 < c2) : (c2 < c1);
  return q1.time < q2.time;
}
```

All updates are stored elsewhere using timestamp.

{% embed url="https://www.spoj.com/problems/XXXXXXXX/" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = int64_t;
const int N = 50005;
const int bloc = 1500;
int dn[N];
ll ret,m[N];
int cnt[N];
struct query{
  int l,r,t,idx;
  query() = default;
  query(int _l,int _r,int _t,int _idx) : l(_l), r(_r), t(_t), idx(_idx) {}
  ~query() = default;
  bool operator<(const query& q){
    if(dn[l] != dn[q.l]) return l < q.l;
    if(dn[r] != dn[q.r]) return r < q.r;
    return t < q.t;
  }
};
struct update{
  int idx,oldval,newval;
  update() = default;
  update(int _idx,int _oldval,int _newval) : idx(_idx), oldval(_oldval), newval(_newval) {}
  ~update() = default;
};
inline void add(int val){
  ++cnt[val];
  if(cnt[val] == 1) ret += m[val];
}
inline void remove(int val){
  --cnt[val];
  if(cnt[val] == 0) ret -= m[val];
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int n;
  cin >> n;
  vector<int> a(n);
  map<int,int> midx;
  for(int i = 0; i < n; ++i){
    cin >> a[i];
    midx[a[i]];
    dn[i] = i/bloc;
  }
  int pos = 0;
  for(auto& p : midx){
    m[pos] = p.first;
    midx[p.first] = pos;
    ++pos;
  }
  for(int i = 0; i < n; ++i) a[i] = midx[a[i]];
  ret = 0ll;
  for(int i = 0; i < n; ++i) add(a[i]);
  int q;
  cin >> q;
  vector<query> queries;
  queries.reserve(q);
  vector<update> updates;
  updates.reserve(q);
  int mo_time = 0,x,y,idx,val;
  char cmd;
  for(int i = 0; i < q; ++i){
    cin >> cmd;
    if(cmd == 'Q'){
      cin >> x >> y;
      --x;
      --y;
      queries.push_back(query(x,y,mo_time,i));
    }
    else{
      cin >> idx >> val;
      --idx;
      if(!midx.count(val)){
        midx[val] = pos;
        m[pos] = val;
        ++pos;
      }
      updates.push_back(update(idx,a[idx],midx[val]));
      remove(a[idx]);
      a[idx] = midx[val];
      add(a[idx]);
      ++mo_time;
    }
  }
  sort(begin(queries),end(queries));
  vector<ll> ans(q,-1);
  int mo_left = 0,mo_right = n-1;
  for(const auto& query : queries){
    while(mo_time < query.t){
      int idx = updates[mo_time].idx;
      int newval = updates[mo_time].newval;
      int oldval = updates[mo_time].oldval;
      if(mo_left <= idx and idx <= mo_right){
        remove(oldval);
        add(newval);
      }
      a[idx] = newval;
      ++mo_time;
    }
    while(mo_time > query.t){
      --mo_time;
      int idx = updates[mo_time].idx;
      int newval = updates[mo_time].newval;
      int oldval = updates[mo_time].oldval;
      if(mo_left <= idx and idx <= mo_right){
        remove(newval);
        add(oldval);
      }
      a[idx] = oldval;
    }
    while(mo_right < query.r) ++mo_right, add(a[mo_right]);
    while(mo_left > query.l) --mo_left, add(a[mo_left]);
    while(mo_right > query.r) remove(a[mo_right]), mo_right--;
    while(mo_left < query.l) remove(a[mo_left]), mo_left++;
    ans[query.idx] = ret;
  }
  for(ll i : ans){
    if(i > -1) cout << i << '\n';
  }
  return 0;
}
```

