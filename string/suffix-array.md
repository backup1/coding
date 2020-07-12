# Suffix Array

[https://codeforces.com/edu/course/2/lesson/2/2/practice/contest/269103/problem/A](https://codeforces.com/edu/course/2/lesson/2/2/practice/contest/269103/problem/A)

```cpp
#include <bits/stdc++.h>
using namespace std;
void count_sort(vector<int>& p,vector<int>& c){
  int n = p.size();
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
void suffix_array(string& s,vector<int>& p){
  s.push_back('$');
  int n = s.size();
  p.resize(n);
  vector<int> c(n);
  vector<pair<char,int>> v(n);
  for(int i = 0; i < n; ++i) v[i] = {s[i],i};
  sort(begin(v),end(v));
  for(int i = 0; i < n; ++i) p[i] = v[i].second;
  // c[p[0]] = 0;
  for(int i =1; i < n; ++i){
    if(v[i].first == v[i-1].first) c[p[i]] = c[p[i-1]];
    else c[p[i]] = c[p[i-1]]+1;
  }
  int delta = 1;
  while(delta < n){
    for(int i = 0; i < n; ++i) p[i] = (p[i]+n-delta)%n;
    count_sort(p,c);
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
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  string s;
  cin >> s;
  vector<int> p;
  suffix_array(s,p);
  for(int i : p) cout << i << ' ';
  return 0;
}
```

