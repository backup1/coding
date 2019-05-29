# initialization

## Initialization in if or switch statement

```cpp
vector<int> v = {2,3,5,7};
if(int a = *min_element(begin(v),end(v)); a > 1)
  cout << a << " > 1";
else
  cout << a << " <= 1";
```

{% hint style="info" %}
Attention: When initializing a vector using auto, you may not get a vector. By default, C++ compiler will consider it as of type initializer\_list&lt;type&gt;.
{% endhint %}

```cpp
auto v = {1,2,3}; // v is of type initializer_list<int>
```

