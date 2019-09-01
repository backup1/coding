# Compilation

You can specify your C++ version via the option -std=c++11, -std=c++14 or -std=c++17. To show all compilation warnings, you can use -Wall.

```text
$ g++ file.cpp -std=c++17 -Wall
```

Sometimes compilation failed because you forget to put the return type of a function. Normal options of the compilation will not point it out. This is painful to waste a lot of time for such silly mistake. Thus better to compile with as many as available options, e.g.

```bash
$ g++ file.cpp -Wall -Wextra -Wpedantic -std=c++14
```

