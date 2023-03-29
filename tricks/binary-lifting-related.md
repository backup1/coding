# Binary lifting related

#### Baltic 2015 Editor

editorial : [https://bits-and-bytes.me/2020/03/17/BtOI-2015-Editor/](https://bits-and-bytes.me/2020/03/17/BtOI-2015-Editor/)

```cpp
#include <bits/stdc++.h>
#define FOR(i, x, y) for (int i = x; i < y; i++)
typedef long long ll;
using namespace std;

int anc[300001][20], mn[300001][20], ans[300001];

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    int n;
    cin >> n;
    FOR(i, 1, n + 1) {
        int x;
        cin >> x;
        if (x > 0) {
            ans[i] = x;
            anc[i][0] = i;
        } else {
            x = -x;
            int curr = i - 1;
            for (int j = 19; ~j; j--) if (mn[curr][j] >= x) curr = anc[curr][j];
            anc[i][0] = curr - 1;
            ans[i] = ans[curr - 1];
            mn[i][0] = x;
            FOR(j, 1, 20) {
                anc[i][j] = anc[anc[i][j - 1]][j - 1];
                mn[i][j] = min(mn[i][j - 1], mn[anc[i][j - 1]][j - 1]);
            }
        }
    }
    FOR(i, 1, n + 1) cout << ans[i] << '\n';
    return 0;
}
```
