---
description: introduced in C++11
---

# lambda expression

## The scope:

* \[ \] : do not capture anything
* \[=\] : capture local objects by value
* \[&\] : capture local objects by reference
* \[this\] : capture this pointer by value
* \[a,&b\] : capture a by value and b by reference

```cpp
vector<int> v = {2,3,5,7,11,13,17};
int d = 23;
for_each(begin(v),end(v),[&d](int & val){
  val *= d;
});
```

