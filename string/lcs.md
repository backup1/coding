# LCP

[https://codeforces.com/edu/course/2/lesson/2/4/practice/contest/269119/problem/A](https://codeforces.com/edu/course/2/lesson/2/4/practice/contest/269119/problem/A)

```cpp
#include <bits/stdc++.h>
using namespace std;
int n;
string s;
vector<int> p,c,lcp;
void count_sort(){
  vector<int> cnt(n),pos(n);
  for(int x : c) ++cnt[x];
  for(int i = 1; i < n; ++i) pos[i] = pos[i-1] + cnt[i-1];
  vector<int> p_new(n);
  for(int x : p){
    int i = c[x];
    p_new[pos[i]] = x;
    pos[i]++;
  }
  p = p_new;
}
void suffix_array(){
  vector<pair<char,int>> v(n);
  for(int i = 0; i < n; ++i) v[i] = {s[i],i};
  sort(begin(v),end(v));
  for(int i = 0; i < n; ++i) p[i] = v[i].second;
  c[p[0]] = 0;
  for(int i = 1; i < n; ++i){
    if(v[i].first == v[i-1].first) c[p[i]] = c[p[i-1]];
    else c[p[i]] = c[p[i-1]]+1;
  }
  int delta = 1;
  while(delta < n){
    for(int i = 0; i < n; ++i) p[i] = (p[i]+n-delta)%n;
    count_sort();
    vector<int> c_new(n);
    c_new[p[0]] = 0;
    pair<int,int> prev = {c[p[0]],c[(p[0]+delta)%n]};
    for(int i = 1; i < n; ++i){
      pair<int,int> now = {c[p[i]],c[(p[i]+delta)%n]};
      if(now == prev) c_new[p[i]] = c_new[p[i-1]];
      else c_new[p[i]] = c_new[p[i-1]]+1;
      prev = now;
    }
    c = c_new;
    delta <<= 1;
  }
}
void LCP(){
  int k = 0;
  for(int i = 0; i < n-1; ++i){
    int pi = c[i];
    int j = p[pi-1];
    // lcp[i] = lcp(s[i,...],s[j,...])
    while(s[i+k] == s[j+k]) ++k;
    lcp[i] = k;
    k = max(k-1,0);
  }
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  cin >> s;
  s.push_back('\1');
  n = s.size();
  p.resize(n);
  c.resize(n);
  suffix_array();
  for(int i : p) cout << i << ' ';
  cout << '\n';
  lcp.resize(n-1);
  LCP();
  for(int i = 1; i < n; ++i) cout << lcp[p[i]] << ' ';
  return 0;
}
```

delimiter and ending character should be chosen wisely, e.g. '\#' as delimiter and '$' as ending character is a very bad choice because '\#' &lt; '$'.

[https://codeforces.com/edu/course/2/lesson/2/5/practice/contest/269656/problem/B](https://codeforces.com/edu/course/2/lesson/2/5/practice/contest/269656/problem/B)

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
ll n;
string s;
vector<ll> p,c,lcp;
void count_sort(){
  vector<ll> cnt(n),pos(n);
  for(ll x : c) ++cnt[x];
  for(ll i = 1; i < n; ++i) pos[i] = pos[i-1] + cnt[i-1];
  vector<ll> p_new(n);
  for(ll x : p){
    ll i = c[x];
    p_new[pos[i]] = x;
    pos[i]++;
  }
  p = p_new;
}
void suffix_array(){
  vector<pair<char,ll>> v(n);
  for(ll i = 0; i < n; ++i) v[i] = {s[i],i};
  sort(begin(v),end(v));
  for(ll i = 0; i < n; ++i) p[i] = v[i].second;
  c[p[0]] = 0;
  for(ll i = 1; i < n; ++i){
    if(v[i].first == v[i-1].first) c[p[i]] = c[p[i-1]];
    else c[p[i]] = c[p[i-1]]+1;
  }
  ll delta = 1;
  while(delta < n){
    for(ll i = 0; i < n; ++i) p[i] = (p[i]+n-delta)%n;
    count_sort();
    vector<ll> c_new(n);
    c_new[p[0]] = 0;
    pair<ll,ll> prev = {c[p[0]],c[(p[0]+delta)%n]};
    for(ll i = 1; i < n; ++i){
      pair<ll,ll> now = {c[p[i]],c[(p[i]+delta)%n]};
      if(now == prev) c_new[p[i]] = c_new[p[i-1]];
      else c_new[p[i]] = c_new[p[i-1]]+1;
      prev = now;
    }
    c = c_new;
    delta <<= 1;
  }
}
void LCP(){
  ll k = 0;
  for(ll i = 0; i < n-1; ++i){
    ll pi = c[i];
    ll j = p[pi-1];
    // lcp[i] = lcp(s[i,...],s[j,...])
    while(s[i+k] == s[j+k]) ++k;
    lcp[i] = k;
    k = max(k-1,0ll);
  }
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  string s1,t1;
  cin >> s1 >> t1;
  ll ns = s1.size();
  s = s1;
  s.push_back('\2');
  s += t1;
  s.push_back('\1');
  n = s.size();
  p.resize(n);
  c.resize(n);
  suffix_array();
  lcp.resize(n-1);
  LCP();
  ll len = -1,beg = 0;
  for(ll i = 0; i < n-1; ++i){
    if(i == ns) continue;
    ll pi = c[i];
    ll j = p[pi-1];
    if((i > ns and j < ns) or (i < ns and j > ns)){
      if((len < lcp[i]) or (len == lcp[i] and pi < c[beg])){
        len = lcp[i];
        beg = i;
      }
    }
  }
  cout << s.substr(beg,len);
  return 0;
}
```

