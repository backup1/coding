# STL utilities

## all\_of, any\_of and none\_of

```cpp
vector<int> v = {1,2,3,4,5,6};
bool aDivBy2 = all_of(begin(v),end(v),[](int x){
  return !(x&1);
}); // false
bool aDivBy3 = any_of(begin(v),end(v),[](int x){
  return x%3 == 0;
}); // true
bool aDivBy6 = none_of(begin(v),end(v),[](int x){
  return x%6 == 0;
}); // false
```

## for\_each

```cpp
vector<int> v = {1,2,3,4,5,6};
for_each(begin(v),end(v),[](int& x){
  x *= x;
});
```

## count and count\_if



