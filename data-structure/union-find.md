# Union Find

## Questions

{% embed url="https://atcoder.jp/contests/abc126/tasks/abc126\_e" %}

```cpp
#include <bits/stdc++.h>
using namespace std;
int getComp(vector<int>& comp,int i){
  if(comp[i] != i) comp[i] = getComp(comp,comp[i]);
  return comp[i];
}
void merge(vector<int>& comp,vector<int>& level,int a,int b){
  a = getComp(comp,a);
  b = getComp(comp,b);
  if(level[a] > level[b]) swap(a,b);
  else if(level[a] == level[b]) ++level[b];
  comp[a] = b;
}
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int n,m,x,y,z;
  cin >> n >> m;
  vector<int> comp(n+1),level(n+1);
  iota(begin(comp),end(comp),0);
  while(m--){
    cin >> x >> y >> z;
    merge(comp,level,x,y);
  }
  unordered_set<int> st;
  for(int i = 1; i <= n; ++i) st.insert(getComp(comp,i));
  cout << st.size();
  return 0;
}
```

