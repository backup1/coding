# base 16

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
        int a;
        cin >> a;
        // display in hexa - base 16
        cout << hex << showbase;
        cout << "hexa : " << a << endl;
        // display in decimal - base 10
        cout << noshowbase << dec;
        cout << "decimal : " << a << endl;
        return 0;
}
```

```bash
$ ./a.exe
168
hexa : 0xa8
decimal : 168
```
