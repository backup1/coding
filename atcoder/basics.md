# Basics

This section is not for an active competitive programmer. But more for a veteran programmer.

The AtCoder library was introduced here : [https://codeforces.com/blog/entry/82400](https://codeforces.com/blog/entry/82400)

You should write down them all if your purpose is to win some medals in a serious competition such as IOI.

The github repository of this library is available here : [https://github.com/atcoder/ac-library/tree/master/](https://github.com/atcoder/ac-library/tree/master/)

A similar repository from tourist could be found here : [https://github.com/the-tourist/algo](https://github.com/the-tourist/algo) which does not contain too much README doc, unfortunately. So that's why we would like to use the AtCoder one.

The template will look more like:

```cpp
#include <bits/stdc++.h>
#include <atcoder/all>
using namespace std;
using namespace atcoder;
using ll = int64_t;
mt19937 rng(chrono::steady_clock::now().time_since_epoch().count());
const ll mod = 1e9+7;
const ll inf = (LLONG_MAX>>1);
int main(){
   ios::sync_with_stdio(false);
   cin.tie(nullptr);
   cout << fixed;
   cout.precision(10);
   // TODO
   return 0;
}
```

