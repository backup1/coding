# Array

In C++, arrays initialized locally using either the default syntax \(i.e. `int arr[10];` \) or the array class are initialized to random numbers, because C++ doesn't have built-in memory management. In order to initialize an array to zero, you have several options:

* Use a for loop \(or nested for loops\).
* Declare the array globally.
* Add braces at the end of the declaration \(i.e. **`int arr[10]{};`** \). See [LearnCpp](https://www.learncpp.com/cpp-tutorial/62-arrays-part-ii/) for more information regarding this syntax.
* Use a built-in function such as [`memset(arr, 0, sizeof arr)`](http://www.cplusplus.com/reference/cstring/memset/) or [`std::fill(arr, arr+10, 0)`](http://www.cplusplus.com/reference/algorithm/fill/).



