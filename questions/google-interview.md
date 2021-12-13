# Google interview

**flatten iterators** : [https://techdevguide.withgoogle.com/resources/former-interview-question-flatten-iterators/](https://techdevguide.withgoogle.com/resources/former-interview-question-flatten-iterators/#!)

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

**find longest word** : [https://techdevguide.withgoogle.com/resources/former-interview-question-find-longest-word/](https://techdevguide.withgoogle.com/resources/former-interview-question-find-longest-word/#!)

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

encode and decode : [https://techdevguide.withgoogle.com/resources/former-interview-question-compression-and-decompression](https://techdevguide.withgoogle.com/resources/former-interview-question-compression-and-decompression/#!)/

encode : [https://massivealgorithms.blogspot.com/2016/12/leetcode-471-encode-string-with.html](https://massivealgorithms.blogspot.com/2016/12/leetcode-471-encode-string-with.html)

decode : [https://massivealgorithms.blogspot.com/2016/09/leetcode-394-decode-string.html](https://massivealgorithms.blogspot.com/2016/09/leetcode-394-decode-string.html)

**lake volume** : [https://techdevguide.withgoogle.com/resources/former-interview-question-volume-of-lakes/](https://techdevguide.withgoogle.com/resources/former-interview-question-volume-of-lakes/#!)

```cpp
#include <bits/stdc++.h>
using namespace std;

// O(n)
int getVolume(vector<int> height){
  int sz = height.size();
  if(sz <= 2) return 0;
  int hmax = height[0],idx = 0;
  for(int i = 1; i < sz; ++i){
    if(height[i] > hmax){
      hmax = height[i];
      idx = i;
    }
  }
  int h = height[0], ret = 0;
  for(int i = 1; i < idx; ++i){
    if(height[i] < h) ret += h-height[i];
    else h = height[i];
  }
  h = height.back();
  for(int i = sz-2; i > idx; --i){
    if(height[i] < h) ret += h-height[i];
    else h = height[i];
  }
  return ret;
}

int main(){
  vector<int> height = {1,3,2,4,1,3,1,4,5,2,2,1,4,2,2};
  cout << getVolume(height) << endl; // 15
  return 0;
}
```

**word squares** : [https://techdevguide.withgoogle.com/resources/former-interview-question-word-squares/](https://techdevguide.withgoogle.com/resources/former-interview-question-word-squares/#!)

```cpp
#include <bits/stdc++.h>
using namespace std;
using vecstr = vector<string>;

class WordSquares{
private:
  vecstr _words;
  unordered_map<string,vecstr> _prefixLookup;
  vector<vecstr> _result;

  void makePrefixLookup(){
    for(const string& word : _words){
      for(int i = 0; i < word.size(); ++i){
        _prefixLookup[word.substr(0,i)].emplace_back(word);
      }
    }
  }

  void collectWordSquares(const vecstr& psol){
    if(psol.size() == _words[0].size()){
      _result.push_back(psol);
      return;
    }
    vecstr cands = getCandidates(psol);
    for(const string& word : cands){
      vecstr npsol = psol;
      npsol.push_back(word);
      collectWordSquares(npsol);
    }
  }

  vecstr getCandidates(const vecstr& psol){
    int pos = psol.size();
    string prefix = "";
    for(auto& s : psol){
      prefix.push_back(s[pos]);
    }
    return _prefixLookup[prefix];
  }

public:
  // T(n,k) = N * ( T(n-1,k-1) + k )
  vector<vecstr> getWordSquares(const vecstr& words){
    _words = words;
    makePrefixLookup();
    collectWordSquares(vecstr());
    return move(_result);
  }
};

int main(){
  vecstr words = {"AREA","BALL","DEAR","LADY","LEAD","YARD"};
  for(vecstr& vs : WordSquares().getWordSquares(words)){
    for(string& s : vs) cout << s << '\n';
    cout << '\n';
  }
  return 0;
}
```
