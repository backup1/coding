# Indexed Set

## Problems

### UVA \#10539

{% embed url="https://uva.onlinejudge.org/index.php?option=onlinejudge&page=show\_problem&problem=1480" %}

```cpp
#include <bits/stdc++.h>
#include <ext/pb_ds/assoc_container.hpp>
using namespace std;
using namespace __gnu_pbds;
using ull = unsigned long long;
using indexed_set = tree<ull,null_type,less<ull>,rb_tree_tag,tree_order_statistics_node_update>;
indexed_set aprimes;
const ull N = 1000000;
const ull N2 = N*N;
vector<bool> isprime(N+1,true);
void init(){
  isprime[0] = false;
  isprime[1] = false;
  for(ull i = 2; i <= N; ++i){
    if(isprime[i]){
      for(ull j = 2*i; j <= N; j += i){
        isprime[j] = false;
      }
      ull n = i*i;
      while(n <= N2){
        aprimes.insert(n);
        n *= i;
      }
    }
  }
}
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  init();
  int N;
  ull low,high;
  cin >> N;
  while(N--){
    cin >> low >> high;
    int lown = aprimes.order_of_key(low);
    int highn = aprimes.order_of_key(high);
    cout << highn - lown << '\n';
  }
  return 0;
}
```

