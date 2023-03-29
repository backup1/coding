# Alien Trick / Lambda Optim

introduction : [http://www.serbanology.com/vault/The%20Trick%20From%20Aliens](http://www.serbanology.com/vault/The%20Trick%20From%20Aliens)

another intro : [https://mamnoonsiam.github.io/posts/attack-on-aliens.html](https://mamnoonsiam.github.io/posts/attack-on-aliens.html)

(kind of binary search on slope)

#### Singapor NOI 2019 Feast

official editorial : [https://github.com/noisg/sg\_noi\_archive/blob/master/2019/solution/NOI\_2019\_Solution.pdf](https://github.com/noisg/sg\_noi\_archive/blob/master/2019/solution/NOI\_2019\_Solution.pdf)

```cpp
#include <bits/stdc++.h>
#define FOR(i, x, y) for (int i = x; i < y; i++)
typedef long long ll;
using namespace std;

ll a[300001], dp[300001];
int cnt[300001];

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    ll n, k;
    cin >> n >> k;
    FOR(i, 1, n + 1) cin >> a[i];

    ll l = 0, r = 3e14, ans = 0;
    while (l != r) {
        ll mid = (l + r) / 2;

        ll local_max = 0, local_max_cnt = 0;
        FOR(i, 1, n + 1) {
            local_max += a[i];
            if (dp[i - 1] >= local_max - mid) {
                dp[i] = dp[i - 1];
                cnt[i] = cnt[i - 1];
            } else {
                dp[i] = local_max - mid;
                cnt[i] = local_max_cnt + 1;
            }

            if (local_max < dp[i - 1]) {
                local_max = dp[i - 1];
                local_max_cnt = cnt[i - 1];
            }
        }

        if (cnt[n] <= k) {
            r = mid;
            ans = max(ans, dp[n] + mid * cnt[n]);
        } else
            l = mid + 1;
    }

    cout << ans;
    return 0;
}
```
