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

{% embed url="https://www.spoj.com/problems/AROPE/" %}

```cpp
#include <bits/stdc++.h>
#include <ext/rope>
using namespace std;
using namespace __gnu_cxx;
int main(){
  ios::sync_with_stdio(0);
  cin.tie(0);
  string s;
  cin >> s;
  rope<char> S = s.c_str();
  int Q,cmd,x,y;
  cin >> Q;
  while(Q--){
    cin >> cmd;
    if(cmd == 1){
      cin >> x >> y;
      rope<char> tmp = S.substr(x,y-x+1);
      S.erase(x,y-x+1);
      S = tmp + S;
    }
    else if(cmd == 2){
      cin >> x >> y;
      rope<char> tmp = S.substr(x,y-x+1);
      S.erase(x,y-x+1);
      S = S + tmp;
    }
    else{
      cin >> y;
      cout << S[y] << '\n';
    }
  }
  return 0;
}
```

