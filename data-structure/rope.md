# Rope

API : [http://www.martinbroadhurst.com/stl/Rope.html](http://www.martinbroadhurst.com/stl/Rope.html)

{% embed url="https://www.codechef.com/problems/TAEDITOR" %}

```cpp
#include <bits/stdc++.h>
#include <ext/rope>
using namespace std;
using namespace __gnu_cxx;
using ll = long long;
const ll MOD = 1e9+7;
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  int T,i,len;
  char cmd;
  string x;
  rope<char> S;
  cin >> T;
  while(T--){
    cin >> cmd;
    if(cmd == '+'){
      cin >> i >> x;
      rope<char> R = x.c_str();
      S.insert(i,R.begin(),R.end());
    }
    else {
      cin >> i >> len;
      cout << S.substr(i-1,len) << '\n';
    }
  }
  return 0;
}
```

