# Fenwick Tree

Given an array of length n, performing in O\(log n\) the queries:

* updating an element;
* sum elements on interval

API : [https://github.com/atcoder/ac-library/blob/master/document\_en/fenwicktree.md](https://github.com/atcoder/ac-library/blob/master/document_en/fenwicktree.md)

```cpp
// constructor, all initialized to 0, O(n)
// T = int, uint, ll, ull or modint
// 0 <= n <= 1e8
fenwick_tree<T> fw(int n)
// add : a[p] += x (0 <= p < n) O(log n)
void fw.add(int p, T x)
// sum : a[l] + ... + a[r-1] (0 <= l <= r <= n) O(log n)
// if overflowed, return result % 1ebit
T fw.sum(int l, int r)
```

Example question : [https://atcoder.jp/contests/practice2/tasks/practice2\_b](https://atcoder.jp/contests/practice2/tasks/practice2_b)

```cpp
#include <bits/stdc++.h>
#include <atcoder/all>
using namespace std;
using namespace atcoder;
using ll = long long;
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int n,q;
  cin >> n >> q;
  fenwick_tree<ll> fw(n);
  for(int i = 0; i < n; ++i){
    ll a;
    cin >> a;
    fw.add(i,a);
  }
  while(q--){
    int op;
    cin >> op;
    if(op == 0){
      int pos;
      ll val;
      cin >> pos >> val;
      fw.add(pos,val);
    }
    else{
      int l,r;
      cin >> l >> r;
      cout << fw.sum(l,r) << '\n';
    }
  }
  return 0;
}
```

