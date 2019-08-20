# Trie / Prefix Tree

## Questions

{% embed url="https://codeforces.com/problemset/problem/271/D" %}

```cpp
    #include <bits/stdc++.h>
    using namespace std;
    struct pt{
      pt* subs[26];
      pt(){
        for(int i = 0; i < 26; ++i) subs[i] = nullptr;
      }
    };
    int dfs(pt* root){
      if(root == nullptr) return 0;
      int ans = 1;
      for(int i = 0; i < 26; ++i) ans += dfs(root->subs[i]);
      return ans;
    }
    int main(){
      ios::sync_with_stdio(false);
      cin.tie(nullptr);
      string s,t;
      int k;
      cin >> s >> t >> k;
      vector<bool> bad(26);
      for(int i = 0; i < 26; ++i){
        if(t[i] == '0') bad[i] = true;
      }
      pt *root = new pt();
      for(int i = 0; i < s.size(); ++i){
        int cnt = 0;
        pt *node = root;
        for(int j = i; j < s.size(); ++j){
          int idx = s[j]-'a';
          if(bad[idx]) ++cnt;
          if(cnt > k) break;
          if(node->subs[idx] == nullptr){
            node->subs[idx] = new pt();
          }
          node = node->subs[idx];
        }
      }
      int ans = dfs(root);
      cout << ans-1;
      return 0;
    }
```



