# USACO template



```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
   ios::sync_with_stdio(false);
   cin.tie(nullptr);
   cout.tie(nullptr);
   freopen("output.out","w",stdout);
   freopen("input.in","r",stdin);
   // TODO
   return 0;
}
```

```cpp
#include <bits/stdc++.h>
using namespace std;

void setIO(string name = "") { // name is nonempty for USACO file I/O
  ios_base::sync_with_stdio(0); cin.tie(0); // see Fast Input & Output
  // alternatively, cin.tie(0)->sync_with_stdio(0);
  if (sz(name)) {
    freopen((name+".in").c_str(), "r", stdin); // see Input & Output
    freopen((name+".out").c_str(), "w", stdout);
  }
}

int main() {
  setIO("question");
  // TODO
  return 0;
}
```

## USACO training template

```cpp
/*
ID: my_id
PROG: question
LANG: C++11
*/
#include <bits/stdc++.h>
using namespace std;
const char endl = '\n';
int main(){
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  freopen("question.in","r",stdin);
  freopen("question.out","w",stdout);
  // TODO
  return 0;
}
```

