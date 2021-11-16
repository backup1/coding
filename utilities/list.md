# list

**list **has forward iterator and backward iterator, while **forward\_list** has only forward iterator

vector has also **emplace(iterator,value)** method, but it will push all elements to its right starting from the position of the iterator

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
  list<int> myList{1,2,3,4,5};
  myList.emplace_front(6);
  myList.emplace_back(7);
  auto forwardIter = begin(myList);
  ++forwardIter;
  ++forwardIter;
  myList.emplace(forwardIter,9);
  auto reverseIter = end(myList);
  --reverseIter;
  --reverseIter;
  --reverseIter;
  myList.emplace(reverseIter,8);
  for(auto&& number : myList) cout << number << endl;
  return 0;
}
```

```bash
$ ./a.exe
6
1
9
2
3
8
4
5
7
```





















