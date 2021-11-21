# interview question from web

strStr : [https://leetcode.com/problems/implement-strstr/](https://leetcode.com/problems/implement-strstr/)

```cpp
int strStr(string haystack, string needle) {
    int n = needle.size();
    if(n == 0) return 0;
    vector<int> pos(n,-1);
    for(int i = 1; i < n; ++i){
        int pre = i-1;
        while(pre >= 0){
            if(needle[pos[pre]+1] == needle[i]){
                pos[i] = pos[pre]+1;
                break;
            }
            pre = pos[pre];
        }
    }
    int idx = -1;
    for(int i = 0; i < haystack.size(); ++i){
        char c = haystack[i];
        while(c != needle[idx+1]){
            if(idx == -1) break;
            idx = pos[idx];
        }
        if(c == needle[idx+1]) ++idx;
        if(idx == n-1) return i-n+1;
    }
    return -1;
}
```
