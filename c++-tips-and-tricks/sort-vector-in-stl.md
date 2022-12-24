# sort vector in STL

sort vector in reversed order:

```cpp
// c++14
std::sort(numbers.begin(), numbers.end(), std::greater<>());

// c++17
std::sort(numbers.begin(), numbers.end(), std::greater{});

// c++20
std::ranges::sort(numbers, std::ranges::greater());
```
