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
  s.push_back('$');
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

