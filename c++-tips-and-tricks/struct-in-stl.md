# struct in STL

question : [https://leetcode.com/contest/biweekly-contest-67/problems/sequentially-ordinal-rank-tracker/](https://leetcode.com/contest/biweekly-contest-67/problems/sequentially-ordinal-rank-tracker/)

```cpp
#include <bits/stdc++.h>
using namespace std;
struct node{
    int rank;
    string name;
    bool operator<(const node& n2) const{
        if(rank != n2.rank) return rank > n2.rank;
        return name < n2.name;
    }
};
using st = set<node>;
class SORTracker {
public:
    int qt = 0;
    st ret,cand;
    SORTracker() {
    }
    
    void add(string name, int score) {
        cand.emplace(node{score,name});
    }
    
    string get() {
        ++qt;
        while(ret.size() < qt){
            auto it = begin(cand);
            ret.emplace(node{it->rank,it->name});
            cand.erase(it);
        }
        while(true){
            if(cand.empty()) break;
            bool ok = true;
            auto it1 = end(ret);
            --it1;
            auto it2 = begin(cand);
            if(*it1 < *it2) break;
            cand.emplace(node{it1->rank,it1->name});
            ret.emplace(node{it2->rank,it2->name});
            ret.erase(it1);
            cand.erase(it2);
        }
        auto it = end(ret);
        --it;
        return it->name;
    }
};

/**
 * Your SORTracker object will be instantiated and called as such:
 * SORTracker* obj = new SORTracker();
 * obj->add(name,score);
 * string param_2 = obj->get();
 */
```
