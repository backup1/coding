# DSU \(Disjoint Set Union\)

API : [https://github.com/atcoder/ac-library/blob/master/document\_en/dsu.md](https://github.com/atcoder/ac-library/blob/master/document_en/dsu.md)

```cpp
// constructor (0 <= n <= 1e8) O(n)
// 0 <= a < n for all nodes a
dsu d(int n)
// merge : add edge a--b
// return component id
int d.merge(int a,int b)
// same : true if a and b are in the same component
bool d.same(int a,int b)
// leader : return the component id
int d.leader(int a)
// size : the size of the component containing a
int d.size(int a)
// groups : list of connected components O(n)
vector<vector<int>> d.groups()
```

Example question : [https://atcoder.jp/contests/practice2/tasks/practice2\_a](https://atcoder.jp/contests/practice2/tasks/practice2_a)

```cpp
#include <bits/stdc++.h>
#include <atcoder/all>
using namespace std;
using namespace atcoder;
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int n,q;
  cin >> n >> q;
  dsu d(n);
  while(q--){
    int op,u,v;
    cin >> op >> u >> v;
    if(op == 0){
      d.merge(u,v);
    }
    else{
      if(d.same(u,v)) cout << 1 << '\n';
      else cout << 0 << '\n';
    }
  }
  return 0;
}
```

