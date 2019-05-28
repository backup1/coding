# UVa template

```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  int T;
  cin >> T;
  cin.ignore();
  while(T--){
    int ret = 0;
    string line;
    while(getline(cin,line)){
      if(line.empty()){
        cout << ret << '\n';
        if(T) cout << '\n'; // no extra line at the end
        break;
      }
      istringstream iss(line);
      int i;
      string s;
      iss >> i >> s;
      // TODO
    }
  }
  return 0;
}
```

