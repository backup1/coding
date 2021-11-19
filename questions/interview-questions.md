# interview questions

C++ derived class

```cpp
#include <bits/stdc++.h>
using namespace std;
class A {
public:
        virtual void print(){
                cout << 1 << endl;
        }
};
class B : public A {
public:
        virtual void print(){
                cout << 2 << endl;
        }
};
int main(){
        A* obj = new B();
        obj->print(); // output : 2
        return 0;
}
```

difference between C++ metaprogramming and Golang metaprogramming

\=> there is no metaprogramming in Golang !
