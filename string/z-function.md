# z-function

LeetCode question : [https://leetcode.com/problems/sum-of-scores-of-built-strings/](https://leetcode.com/problems/sum-of-scores-of-built-strings/)

Reference on z-function : [https://cp-algorithms.com/string/z-function.html](https://cp-algorithms.com/string/z-function.html)

```cpp
class Solution {
    vector<int> z_function(string s) {
        int n = s.size(), l = 0, r = 0;
        vector<int> z(n);
        for(int i = 1; i < n; ++i) {
            if(i <= r) z[i] = min(r - i + 1, z[i - l]);
            while(i + z[i] < n and s[z[i]] == s[i + z[i]]) ++z[i];
            if(i + z[i] - 1 > r){
                l = i;
                r = i+z[i]-1;
            }
        }
        return z;
    }
public:
    long long sumScores(string s) {
        long long ret = s.size();
        for(long long i : z_function(s)) ret += i;
        return ret;
    }
};
```

```python
class Solution:
    def sumScores(self, s):
        def z_function(s):
            n = len(s)
            z = [0] * n
            l, r = 0, 0
            for i in range(1, n):
                if i <= r:
                    z[i] = min(r - i + 1, z[i - l])
                while i + z[i] < n and s[z[i]] == s[i + z[i]]:
                    z[i] += 1
                if i + z[i] - 1 > r:
                    l, r = i, i + z[i] - 1
            return z
        
        return sum(z_function(s)) + len(s)
```
