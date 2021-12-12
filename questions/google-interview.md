# Google interview

flatten iterators : https://techdevguide.withgoogle.com/resources/former-interview-question-flatten-iterators/

```cpp
#include <bits/stdc++.h>
using namespace std;
using it = vector<int>::iterator;

class itflatten{
  deque<it> q;
  set<it> st;
public:
  // O(k)
  itflatten(vector<it>&& itlist,vector<it>&& ends){
    q.assign(begin(itlist),end(itlist));
    for(auto&& iter : ends) st.emplace(iter);
  }
  // O(1)
  it nxt(){
    auto iter = q.front();
    q.pop_front();
    if(!st.count(next(iter))) q.push_back(next(iter));
    return iter;
  }
  // O(1)
  bool hasNxt(){
    return !q.empty();
  }
};

int main(){
  vector<int> v1 = {1,2,3}, v2 = {4,5}, v3 = {6,7,8,9};
  itflatten a{{begin(v1),begin(v2),begin(v3)},{end(v1),end(v2),end(v3)}};
  while(a.hasNxt()){
    cout << *a.nxt() << '\n';
  }
  return 0;
}
```

find longest word : [https://techdevguide.withgoogle.com/resources/former-interview-question-find-longest-word/](https://techdevguide.withgoogle.com/resources/former-interview-question-find-longest-word/#!)

```cpp
#include <bits/stdc++.h>
using namespace std;

// O( len(sample) + \sum_{word in words} len(word) )
string findlongestword(const string& sample,vector<string>& words){
  if(sample.empty() or words.empty()) return " ";
  unordered_map<char,set<pair<string,int>>> mp;
  for(string& word : words){
    if(word.empty()) continue;
    mp[word[0]].emplace(word,0);
  }
  string longest = "";
  for(char c : sample){
    if(!mp.count(c)) continue;
    auto listwords = mp[c];
    for(auto p : listwords){
      string word;
      int len;
      tie(word,len) = p;
      mp[c].erase(p);
      ++len;
      if(len == word.size()){
        if(len > longest.size()) longest = word;
      }
      else mp[word[len]].emplace(word,len);
    }
  }
  return longest;
}

int main(){
  string sample = "abcdppleeee";
  vector<string> words = {"cp","adee","kangourou","leet","apple","cdeee"};
  cout << findlongestword(sample,words) << endl;
  return 0;
}
```
