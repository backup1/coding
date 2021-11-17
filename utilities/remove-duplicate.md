# remove duplicate

ref [https://stackoverflow.com/questions/1041620/whats-the-most-efficient-way-to-erase-duplicates-and-sort-a-vector](https://stackoverflow.com/questions/1041620/whats-the-most-efficient-way-to-erase-duplicates-and-sort-a-vector)

```cpp
sort(begin(v),end(v));
v.erase(unique(begin(v),end(v)),end(v));
```
