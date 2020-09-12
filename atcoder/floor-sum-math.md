# Floor Sum \(Math\)

API : [https://github.com/atcoder/ac-library/blob/master/document\_en/math.md](https://github.com/atcoder/ac-library/blob/master/document_en/math.md)

```cpp
// x^n mod m (0 <= n and 1 <= m) O(log n)
ll pow_mod(ll x, ll n, int m)
// return y such that 0 <= y < m and xy = 1 mod m
// gcd(x,m) = 1 (1 <= m) O(log m)
ll inv_mod(ll x, ll m)
// given 2 vector r, m of length n, find out x such that
// x = r[i] mod m[i] for i = 0, ..., n-1
// return (0,1) if n = 0
// return (0,0) if no solution
// return (y,z) if solution is x = y mod z (0 <= y < z <= lcm(m[i]))
// 1 <= m[i] and lcm(m[i]) is ll, O(lcm(m[i]))
pair<ll,ll> crt(vector<ll> r, vector<ll> m)
// floor sum : 0 <= n <= 1e9, 1 <= m <= 1e9, 0 <= a, b <= m
// sum_{0 <= i < n} floor((a*i+b)/m)
// O(log (n+m+a+b))
ll floor_sum(ll n, ll m, ll a, ll b)
```

Example question : [https://atcoder.jp/contests/practice2/tasks/practice2\_c](https://atcoder.jp/contests/practice2/tasks/practice2_c)

```cpp
#include <bits/stdc++.h>
#include <atcoder/math>
using namespace std;
using namespace atcoder;
using ll = long long;
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int t;
  cin >> t;
  while(t--){
    ll n,m,a,b;
    cin >> n >> m >> a >> b;
    cout << floor_sum(n,m,a,b) << '\n';
  }
  return 0;
}
```

