---
description: (re)introduced since C++11
---

# auto

## Structured binding \(C++17\)

Structured binding is working for array, tuple and data members in a structure. This feature is available since C++17.

```cpp
vector<int> v = {1,2};
// C++17
auto [a,b] = v;
auto& [a_ref,b_ref] = v;
// C++14 or earlier
int a,b;
tie(a,b) = v;
```

## Function declaration \(C++17\)

```cpp
auto merge(const auto& va,const auto& vb){
  auto ret = va;
  for(auto item : vb) ret.push_back(item);
  return ret;
}
```

## lambda expression \(C++14\)

```cpp
sort(begin(v),end(v),[](const auto& a,const auto& b){
  if(a.first != b.first) return a.first > b.first;
  return a.second < b.second;
});
```



