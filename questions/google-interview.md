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

