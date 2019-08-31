# Segment Tree

## Min Segment Tree

```cpp
int getMin(vector<int>& segt,int idx,int left,int right,int L,int R){
  if(L >= R) return inf;
  if(L == left and R == right) return segt[idx];
  int mid = (left+right)/2;
  return min(getMin(segt,2*idx+1,left,mid,L,min(R,mid)),
             getMin(segt,2*idx+2,mid,right,max(L,mid),R));
}
void update(vector<int>& segt,int idx,int left,int right,int pos,int val){
  if(right-left <= 1){
    segt[idx] = val;
    return;
  }
  int mid = (left+right)/2;
  if(pos < mid) update(segt,2*idx+1,left,mid,pos,val);
  else update(segt,2*idx+2,mid,right,pos,val);
  segt[idx] = min(segt[2*idx+1],segt[2*idx+2]);
}
```

## Problems

{% embed url="https://codeforces.com/contest/1187/problem/D" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
const int inf = INT_MAX/2;
int getMin(vector<int>& segt,int idx,int left,int right,int L,int R){
  if(L >= R) return inf;
  if(L == left and R == right) return segt[idx];
  int mid = (left+right)/2;
  return min(getMin(segt,2*idx+1,left,mid,L,min(R,mid)),
             getMin(segt,2*idx+2,mid,right,max(L,mid),R));
}
void update(vector<int>& segt,int idx,int left,int right,int pos,int val){
  if(right-left <= 1){
    segt[idx] = val;
    return;
  }
  int mid = (left+right)/2;
  if(pos < mid) update(segt,2*idx+1,left,mid,pos,val);
  else update(segt,2*idx+2,mid,right,pos,val);
  segt[idx] = min(segt[2*idx+1],segt[2*idx+2]);
}
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  int t;
  cin >> t;
  while(t--){
    int n,a,b;
    cin >> n;
    vector<deque<int>> aidxs(n+1);
    vector<int> segt(4*n+100,inf);
    for(int i = 0; i < n; ++i){
      cin >> a;
      --a;
      aidxs[a].push_back(i);
    }
    for(int i = 0; i < n; ++i){
      if(!aidxs[i].empty())
        update(segt,0,0,n,i,aidxs[i][0]);
    }
    bool done = false;
    for(int i = 0; i < n; ++i){
      cin >> b;
      --b;
      if(done) continue;
      if(aidxs[b].empty() or
         aidxs[b][0] != getMin(segt,0,0,n,0,b+1)){
        cout << "NO\n";
        done = true;
        continue;
      }
      aidxs[b].pop_front();
      int val = inf;
      if(!aidxs[b].empty()) val = aidxs[b][0];
      update(segt,0,0,n,b,val);
    }
    if(!done) cout << "YES\n";
  }
  return 0;
}
```

{% embed url="https://codeforces.com/problemset/problem/292/E" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
vector<vector<int>> v;
int get(vector<pair<int,int>>& segt,int idx,int left,int right,int pos){
  if(segt[idx].first){
    int array,i;
    tie(array,i) = segt[idx];
    return v[array-1][i+pos-left];
  }
  int mid = (left+right)/2;
  if(pos < mid) return get(segt,2*idx+1,left,mid,pos);
  return get(segt,2*idx+2,mid,right,pos);
}
void update(vector<pair<int,int>>& segt,int idx,int left,int right,int L,int R,int array,int beg){
  if(L >= R) return;
  if(L == left and R == right){
    segt[idx] = make_pair(array,beg);
    return;
  }
  int mid = (left+right)/2;
  if(segt[idx].first){
    segt[2*idx+1] = segt[idx];
    segt[2*idx+2] = segt[idx];
    segt[2*idx+2].second += mid-left;
    segt[idx] = make_pair(0,0);
  }
  update(segt,2*idx+1,left,mid,L,min(R,mid),array,beg);
  update(segt,2*idx+2,mid,right,max(L,mid),R,array,beg+max(mid-L,0));
}
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  int n,m,t,x,y,k;
  cin >> n >> m;
  v = vector<vector<int>>(2,vector<int>(n+1));
  for(int i = 1; i <= n; ++i) cin >> v[0][i];
  for(int i = 1; i <= n; ++i) cin >> v[1][i];
  vector<pair<int,int>> segt(4*n+5);
  update(segt,0,1,n+1,1,n+1,2,1);
  while(m--){
    cin >> t;
    if(t == 1){
      cin >> x >> y >> k;
      update(segt,0,1,n+1,y,y+k,1,x);
    }
    else{
      cin >> x;
      cout << get(segt,0,1,n+1,x) << '\n';
    }
  }
  return 0;
}
```

{% embed url="https://codeforces.com/problemset/problem/641/E" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
int getSum(vector<int>& segt,int idx,int left,int right,int L,int R){
  if(L >= R) return 0;
  if(L == left and R == right) return segt[idx];
  int mid = (left+right)/2;
  return getSum(segt,2*idx+1,left,mid,L,min(R,mid))+
         getSum(segt,2*idx+2,mid,right,max(L,mid),R);
}
void update(vector<int>& segt,int idx,int left,int right,int pos,int delta){
  if(right-left <= 1){
    segt[idx] += delta;
    return;
  }
  int mid = (left+right)/2;
  if(pos < mid) update(segt,2*idx+1,left,mid,pos,delta);
  else update(segt,2*idx+2,mid,right,pos,delta);
  segt[idx] = segt[2*idx+1]+segt[2*idx+2];
}
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  int n,a,t,x;
  cin >> n;
  vector<tuple<int,int,int>> cmds;
  map<int,set<int>> ts;
  while(n--){
    cin >> a >> t >> x;
    cmds.emplace_back(a,t,x);
    ts[x].emplace(t);
  }
  map<int,vector<int>> tv;
  for(auto& p : ts) tv[p.first].assign(begin(p.second),end(p.second));
  map<int,map<int,int>> mt;
  map<int,int> ls;
  for(auto& p : tv){
    ls[p.first] = p.second.size();
    for(int i = 0; i < p.second.size(); ++i) mt[p.first][p.second[i]] = i;
  }
  map<int,vector<int>> m;
  for(auto& p : cmds){
    tie(a,t,x) = p;
    if(!m.count(x)) m[x] = vector<int>(4*ls[x]+4);
    if(a == 1) update(m[x],0,0,ls[x],mt[x][t],1);
    else if(a == 2) update(m[x],0,0,ls[x],mt[x][t],-1);
    else cout << getSum(m[x],0,0,ls[x],0,mt[x][t]) << '\n';
  }
  return 0;
}
```

{% embed url="https://codeforces.com/problemset/problem/482/B" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1e5;
int n;
vector<int> t;
void build(){
  for(int i = n-1; i > 0; --i){
    t[i] = t[2*i] + t[2*i+1];
  }
}
int query(int l,int r){
  int res = 0;
  l += n;
  r += n;
  while(l < r){
    if(l&1){
      res += t[l];
      ++l;
    }
    if(r&1){
      res += t[r-1];
      --r;
    }
    l /= 2;
    r /= 2;
  }
  return res;
}
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  int m,l,r,q;
  cin >> n >> m;
  vector<tuple<int,int,int>> qs;
  while(m--){
    cin >> l >> r >> q;
    qs.emplace_back(l-1,r-1,q);
  }
  vector<int> ans(n);
  for(int pos = 0; pos < 30; ++pos){
    t = vector<int>(2*n);
    vector<int> cnt(n+1);
    for(auto& tpl : qs){
      tie(l,r,q) = tpl;
      if(q&(1<<pos)){
        ++cnt[l];
        --cnt[r+1];
      }
    }
    for(int i = 0; i < n; ++i){
      if(i > 0) cnt[i] += cnt[i-1];
      if(cnt[i] > 0){
        t[n+i] = 1;
        ans[i] += (1<<pos);
      }
    }
    build();
    for(auto& tpl : qs){
      tie(l,r,q) = tpl;
      if(q&(1<<pos)){
        if(query(l,r+1) < r-l+1){
          cout << "NO";
          return 0;
        }
      }
      else if(query(l,r+1) == r-l+1){
        cout << "NO";
        return 0;
      }
    }
  }
  cout << "YES\n";
  for(int i : ans) cout << i << ' ';
  return 0;
}
```

{% embed url="https://codeforces.com/problemset/problem/1198/B" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const ll inf = (LLONG_MAX>>2);
vector<ll> segt;
ll get(ll idx,ll left,ll right,ll pos){
  if(right-left <= 1) return segt[idx];
  if(segt[idx] > -inf){
    segt[2*idx+1] = max(segt[2*idx+1],segt[idx]);
    segt[2*idx+2] = max(segt[2*idx+2],segt[idx]);
    segt[idx] = -inf;
  }
  ll mid = (left+right)/2;
  if(pos < mid) return get(2*idx+1,left,mid,pos);
  return get(2*idx+2,mid,right,pos);
}
void update(ll idx,ll left,ll right,ll pos,ll val){
  if(right-left <= 1){
    segt[idx] = val;
    return;
  }
  if(segt[idx] > -inf){
    segt[2*idx+1] = max(segt[2*idx+1],segt[idx]);
    segt[2*idx+2] = max(segt[2*idx+2],segt[idx]);
    segt[idx] = -inf;
  }
  ll mid = (left+right)/2;
  if(pos < mid) update(2*idx+1,left,mid,pos,val);
  else update(2*idx+2,mid,right,pos,val);
}
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  ll n,q,a,op,p,x;
  cin >> n;
  segt = vector<ll>(1000000,-inf);
  for(ll i = 0; i < n; ++i){
    cin >> a;
    update(1,0,n,i,a);
  }
  cin >> q;
  while(q--){
    cin >> op;
    if(op == 1){
      cin >> p >> x;
      update(1,0,n,p-1,x);
    }
    else{
      cin >> x;
      segt[1] = max(segt[1],x);
    }
  }
  for(ll i = 0; i < n; ++i) cout << get(1,0,n,i) << ' ';
  return 0;
}
```

{% embed url="https://codeforces.com/contest/474/problem/F" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
vector<pair<int,int>> segt;
int getGCD(int l,int r){
  int ret = 0;
  while(l <= r){
    if(l&1){
      ret = __gcd(ret,segt[l].first);
      ++l;
    }
    if(r%2 == 0){
      ret = __gcd(ret,segt[r].first);
      --r;
    }
    l >>= 1;
    r >>= 1;
  }
  return ret;
}
int getNb(int l,int r,int gcd){
  int ret = 0;
  while(l <= r){
    if(l&1){
      if(gcd == segt[l].first) ret += segt[l].second;
      ++l;
    }
    if(r%2 == 0){
      if(gcd == segt[r].first) ret += segt[r].second;
      --r;
    }
    l >>= 1;
    r >>= 1;
  }
  return ret;
}
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  int n,t,l,r;
  cin >> n;
  segt = vector<pair<int,int>>(2*n);
  for(int i = n; i < 2*n; ++i){
    cin >> segt[i].first;
    segt[i].second = 1;
  }
  for(int i = n-1; i >= 1; --i){
    segt[i].first = __gcd(segt[2*i].first,segt[2*i+1].first);
    if(segt[i].first == segt[2*i].first) segt[i].second += segt[2*i].second;
    if(segt[i].first == segt[2*i+1].first) segt[i].second += segt[2*i+1].second;
  }
  cin >> t;
  while(t--){
    cin >> l >> r;
    int gcd = getGCD(n+l-1,n+r-1);
    cout << r-l+1-getNb(n+l-1,n+r-1,gcd) << '\n';
  }
  return 0;
}
```

## With index compression

{% embed url="https://www.spoj.com/problems/BGSHOOT/" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
vector<ll> v;
inline ll query(ll b,ll e){
  ll ans = 0;
  while(b <= e){
    if(b%2 == 1) ans = max(ans,v[b++]);
    if(e%2 == 0) ans = max(ans,v[e--]);
    b >>= 1;
    e >>= 1;
  }
  return ans;
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  ll N,Q,a,b;
  cin >> N;
  vector<pair<ll,ll>> vp;
  set<ll> xs;
  for(ll i = 0; i < N; ++i){
    cin >> a >> b;
    vp.emplace_back(a,b);
    xs.insert(a);
    xs.insert(b);
  }
  vector<ll> vxs;
  vxs.assign(begin(xs),end(xs));
  ll n = vxs.size();
  map<ll,ll> mx;
  for(ll i = 0; i < n; ++i) mx[vxs[i]] = i;
  v = vector<ll>(4*n+1);
  for(ll i = 0; i < N; ++i){
    tie(a,b) = vp[i];
    ++v[2*n+2*mx[a]];
    --v[2*n+2*mx[b]+1];
  }
  for(ll i = 2*n+1; i <= 4*n; ++i) v[i] += v[i-1];
  for(ll i = 2*n-1; i > 0; --i) v[i] = max(v[2*i],v[2*i+1]);
  cin >> Q;
  while(Q--){
    cin >> a >> b;
    ll xa = lower_bound(begin(vxs),end(vxs),a)-begin(vxs);
    ll xb = upper_bound(begin(vxs),end(vxs),b)-begin(vxs);
    cout << query(2*n+2*xa-1,2*n+2*xb-1) << '\n';
  }
  return 0;
}
```

## Update on interval and query on points

{% embed url="https://www.spoj.com/problems/UPDATEIT/" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
vector<int> v;
int querySum(int l,int r){
  int ans = 0;
  while(l <= r){
    if(l&1){
      ans += v[l];
      ++l;
    }
    if(r%2 == 0){
      ans += v[r];
      --r;
    }
    l >>= 1;
    r >>= 1;
  }
  return ans;
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int t;
  cin >> t;
  while(t--){
    int n,u,q,l,r,val,idx;
    cin >> n >> u;
    v = vector<int>(2*n+5);
    while(u--){
      cin >> l >> r >> val;
      v[n+l] += val;
      v[n+r+1] -= val;
    }
    for(int i = n-1; i > 0; --i) v[i] = v[2*i] + v[2*i+1];
    cin >> q;
    while(q--){
      cin >> idx;
      cout << querySum(n,n+idx) << '\n';
    }
  }
  return 0;
}
```

## Others

{% embed url="https://www.spoj.com/problems/ILKQUERY2/" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 100005;
vector<int> a(N);
map<int,int> ml;
map<int,vector<int>> m,m2;
inline int getCnt(int val,int left,int right){
  int ans = 0;
  while(left <= right){
    if(left&1){
      ans += m2[val][left];
      ++left;
    }
    if(right%2 == 0){
      ans += m2[val][right];
      --right;
    }
    left >>= 1;
    right >>= 1;
  }
  return ans;
}
inline void updCnt(int val,int idx,int delta){
  while(idx >= 1){
    m2[val][idx] += delta;
    idx >>= 1;
  }
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int n,q,cmd,l,r,k;
  cin >> n >> q;
  for(int i = 0; i < n; ++i){
    cin >> a[i];
    m[a[i]].push_back(i);
  }
  for(auto& p : m){
    int val = p.first;
    ml[val] = p.second.size();
    m2[val] = vector<int>(2*ml[val],1);
    for(int i = ml[val]-1; i > 0; --i){
      m2[val][i] = m2[val][2*i] + m2[val][2*i+1];
    }
  }
  while(q--){
    cin >> cmd;
    if(cmd == 0){
      cin >> l >> r >> k;
      if(!m.count(k)){
        cout << 0 << '\n';
        continue;
      }
      auto it1 = lower_bound(begin(m[k]),end(m[k]),l);
      if(it1 == end(m[k])){
        cout << 0 << '\n';
        continue;
      }
      int left = it1-begin(m[k]);
      int right = upper_bound(begin(m[k]),end(m[k]),r)-begin(m[k]);
      left += ml[k];
      right += ml[k]-1;
      cout << getCnt(k,left,right) << '\n';
    }
    else{
      cin >> r;
      int val = a[r];
      int idx = lower_bound(begin(m[val]),end(m[val]),r)-begin(m[val]);
      idx += ml[val];
      if(m2[val][idx] == 1) updCnt(val,idx,-1);
      else updCnt(val,idx,1);
    }
  }
  return 0;
}
```

{% embed url="https://www.spoj.com/problems/IQUERY/" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = int64_t;
const ll MOD = 1e9+7;
ll n;
vector<ll> mult,bit[17];
inline ll getPow(ll a,ll p){
  ll ret = 1,cp = a;
  while(p){
    if(p&1) ret = (ret*cp)%MOD;
    p >>= 1;
    cp = (cp*cp)%MOD;
  }
  return ret;
}
inline ll getInverse(ll a){
  return getPow(a,MOD-2)%MOD;
}
inline void init(){
  for(ll i = n-1; i > 0; --i){
    ll i1 = (i<<1), i2 = i1+1;
    mult[i] = (mult[i1]*mult[i2])%MOD;
    for(ll j = 0; j < 17; ++j) bit[j][i] = bit[j][i1]+bit[j][i2];
  }
}
inline void update(ll pos,ll val){
  ll posm = pos,oldinv = getInverse(mult[pos]),delta = ((val+1)*oldinv)%MOD;
  do{
    mult[posm] = (mult[posm]*delta)%MOD;
    posm >>= 1;
  } while(posm > 0);
  for(ll j = 0; j < 17; ++j){
    ll posb = pos,oldval = bit[j][posb],newval = (val>>j)&1,delta = newval-oldval;
    if(delta == 0) continue;
    do{
      bit[j][posb] += delta;
      posb >>= 1;
    } while(posb > 0);
  }
}
inline ll getM(ll b,ll e){
  ll ans = 1;
  while(b <= e){
    if(b&1){
      ans = (ans*mult[b])%MOD;
      ++b;
    }
    if(e%2 == 0){
      ans = (ans*mult[e])%MOD;
      --e;
    }
    b >>= 1;
    e >>= 1;
  }
  return (ans+MOD-1)%MOD;
}
inline ll getB(ll b,ll e){
  ll ans = 0;
  for(ll i = 0; i < 17; ++i){
    ll tmp = 0,bi = b,ei = e;
    while(bi <= ei){
      if(bi&1){
        tmp += bit[i][bi];
        ++bi;
      }
      if(ei%2 == 0){
        tmp += bit[i][ei];
        --ei;
      }
      bi >>= 1;
      ei >>= 1;
    }
    ans = (ans+(getPow(2,tmp)-1)*getPow(2,i))%MOD;
  }
  return ans;
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  ll a,b,q;
  cin >> n;
  mult = vector<ll>(2*n);
  for(ll i = 0; i < 17; ++i) bit[i] = vector<ll>(2*n);
  for(ll i = 0; i < n; ++i){
    cin >> a;
    mult[n+i] = a+1;
    for(ll j = 0; j < 17; ++j){
      if((a>>j)&1) bit[j][n+i] = 1;
    }
  }
  init();
  cin >> q;
  char cmd;
  while(q--){
    cin >> cmd >> a >> b;
    if(cmd == 'U'){
      update(n+a-1,b);
    }
    else if(cmd == 'M'){
      cout << getM(n+a-1,n+b-1) << '\n';
    }
    else{
      cout << getB(n+a-1,n+b-1) << '\n';
    }
  }
  return 0;
}
```

