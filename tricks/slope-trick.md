# Slope Trick

introduction : [https://usaco.guide/adv/slope-trick?lang=cpp](https://usaco.guide/adv/slope-trick?lang=cpp)

CSES question : [https://cses.fi/problemset/task/2132](https://cses.fi/problemset/task/2132)

```cpp
#include <bits/stdc++.h>

int main() {
    int n;
    scanf("%d", &n);
    long long ans = 0;
    std::priority_queue<int> slope_change;
    while (n--) {
        int a;
        scanf("%d", &a);
        slope_change.push(a); slope_change.push(a);
        ans += slope_change.top() - a;
        slope_change.pop();
    }
    printf("%lld", ans);
    return 0;
}
```
