# Google interview

**System Design Interview book :** [https://ipfs.io/ipfs/bafykbzacecpbuvilbmokyl2ajs2hbve3qaut4h3vtir6htydecy2h5jaqmy7i?filename=Alex%20Yu%20-%20System%20Design%20Interview\_%20An%20Insider%E2%80%99s%20Guide-Independently%20published%20%282020%29.pdf](https://ipfs.io/ipfs/bafykbzacecpbuvilbmokyl2ajs2hbve3qaut4h3vtir6htydecy2h5jaqmy7i?filename=Alex%20Yu%20-%20System%20Design%20Interview\_%20An%20Insider%E2%80%99s%20Guide-Independently%20published%20%282020%29.pdf)

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

```cpp
#include <bits/stdc++.h>
using namespace std;

string encode(string s){
  int n = s.size();
  vector<vector<string>> dp(n,vector<string>(n,""));
  for(int step = 1; step <= n; ++step){
    for(int i = 0; i+step <= n; ++i){
      string t = s.substr(i,step), replace = "";
      int j = i+step-1;
      dp[i][j] = t;
      auto pos = (t+t).find(t,1);
      if(pos < t.size()){
        replace = to_string(t.size()/pos)+"["+dp[i][i+pos-1]+"]";
        if(replace.size() < t.size()) dp[i][j] = replace;
      }
      for(int k = i; k < j; ++k){
        if(dp[i][k].size()+dp[k+1][j].size() < dp[i][j].size()){
          dp[i][j] = dp[i][k]+dp[k+1][j];
        }
      }
      //cout << "dp[" << i << "][" << j << "] = " << dp[i][j] << endl;
    }c
  }
  return dp[0][n-1];
}

int main(){
  cout << encode("aaaaaabaaaaababcabcabcababababc") << endl;
  return 0;
}
```

decode : [https://massivealgorithms.blogspot.com/2016/09/leetcode-394-decode-string.html](https://massivealgorithms.blogspot.com/2016/09/leetcode-394-decode-string.html)

**decode string :** [https://leetcode.com/problems/decode-string/](https://leetcode.com/problems/decode-string/)

```cpp
string decodeString(string s) {
  string ans = "";
  int n = 0;
  for(int i = 0; i < s.size(); ++i){
    char c = s[i];
    if(c == '['){
      int cnt = 1;
      string inside = "";
      while(cnt > 0){
        ++i;
        if(s[i] == '[') ++cnt;
        else if(s[i] == ']') --cnt;
        if(cnt == 0) break;
        inside.push_back(s[i]);
      }
      string tmp = decodeString(inside);
      for(int j = 0; j < n; ++j) ans += tmp;
      n = 0;
    }
    else if(isdigit(c)) n = 10*n+(c-'0');
    else ans.push_back(c);
  }
  return ans;
}
```

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

**minesweeper** : [https://techdevguide.withgoogle.com/resources/former-interview-question-minesweeper/](https://techdevguide.withgoogle.com/resources/former-interview-question-minesweeper/#!)

```cpp
#include <bits/stdc++.h>
using namespace std;

template<typename T>
class Mine{
public:
  void resize(int rows,int cols){
    _data.resize(rows*cols);
    _rows = rows;
    _cols = cols;
  }
  T& at(int row,int col){
    return _data.at(row*_cols+col);
  }
  int rows(){
    return _rows;
  }
  int cols(){
    return _cols;
  }
private:
  vector<T> _data;
  int _rows = 0;
  int _cols = 0;
};

constexpr int kMine = 9;
vector<pair<int,int>> dirs = { {1,0}, {-1,0}, {0,1}, {0,-1} };

class MineGame{
private:
  struct Spot{
    int val = 0;
    bool visible = false;
  };
  Mine<Spot> game;
public:
  MineGame(int rows,int cols,int nbMines){
    game.resize(rows,cols);
    const int tot = rows*cols;
    if(nbMines > tot){
      cout << "Too many mines!" << endl;
      nbMines = tot;
    }
    for(int i = 0; i < nbMines; ++i){
      game.at(i/cols,i%cols).val = (i < nbMines ? kMine : 0);
    }
    // random shuffle
    for(int i = 0; i < min(nbMines,tot-1); ++i){
      int j = i + (rand()%(tot-i));
      swap(game.at(i/cols,i%cols),game.at(j/cols,j%cols));
    }
    // set value
    for(int x = 0; x < rows; ++x){
      for(int y = 0; y < cols; ++y){
        if(game.at(x,y).val != kMine) continue;
        for(int dx : {-1,0,1}){
          for(int dy : {-1,0,1}){
            int nx = x + dx, ny = y + dy;
            if(0 <= nx and nx < rows and 0 <= ny and ny < cols and game.at(nx,ny).val != kMine){
              game.at(nx,ny).val++;
            }
          }
        }
      }
    }
  }
  bool onClick(int row,int col){
    if(row < 0 or row >= game.rows() or col < 0 or col >= game.cols()) return false;
    if(game.at(row,col).visible == true) return false;
    game.at(row,col).visible = true;
    if(game.at(row,col).val == kMine){
      cout << "BOOM!" << endl;
      return true;
    }
    if(game.at(row,col).val != 0) return false;
    for(auto& dir : dirs){
      onClick(row + dir.first, col + dir.second);
    }
    return false;
  }
  void show(bool showAll){
    for(int i = 0; i < game.rows(); ++i){
      for(int j = 0; j < game.cols(); ++j){
        if(game.at(i,j).visible or showAll){
          cout << game.at(i,j).val << ' ';
        }
        else{
          cout << ". ";
        }
      }
      cout << endl;
    }
    cout << endl;
  }
};

int main(){
  //srand(chrono::steady_clock::now().time_since_epoch().count());
  srand(time(NULL));
  //srand(0);
  MineGame minegame(8,13,7);
  minegame.show(true);
  minegame.onClick(5,1);
  minegame.show(false);
  minegame.onClick(2,6);
  minegame.show(false);
  minegame.onClick(9,3);
  minegame.show(false);
  minegame.onClick(0,0);
  minegame.show(false);
  minegame.onClick(3,5);
  minegame.show(false);
  return 0;
}
```

Flood-it : [https://unixpapa.com/floodit/?sz=14\&nc=5](https://unixpapa.com/floodit/?sz=14\&nc=5)

Each situation is considered as a node in a graph, then (C-1) oriented edges for the next situations.

We can do BFS in this case, but it is still slow.

Doing Dijkstra or A\* with a measure of lower bound. Number of remaining color is ok, but not enough. Counting the max distance to the remaining components is a good choice of this measure.

C++ RVO, what it is ? **return value optimization** (RVO) [https://en.wikipedia.org/wiki/Copy\_elision](https://en.wikipedia.org/wiki/Copy\_elision)
