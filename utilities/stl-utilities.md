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

```cpp
vector<int> v = {1,3,5,7,9,2,3,5,7};
cout << count(begin(v),end(v),3);
cout << count_if(begin(v),end(v),[](int x){
  return !(x&1);
});
```

## find\_if

```cpp
vector<int> v = {1,3,5,7,9,2,3,5,7};
auto it = find_if(begin(v),end(v),[](int x){
  return x%2 == 0;
});
cout << *it;
```

## generate

```cpp

```

