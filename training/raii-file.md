# RAII - file

squelette to start

```cpp
#include <stdio.h>
#include <stdlib.h>

#include <filesystem>
#include <stdexcept>
#include <string>

////////////////////////////////////////////////////////////////////////////////////////////////////
//
// Exercise 1
//
// Implement a RAII wrapper for FILE* that will use the basic C interface in a safe manner.
//
// The file class will allow to open a text file and close it on release.
// As copying FILE* makes no sense, you'll deactivate the copy constructor and assignment
// and provide a correct move constructor and assignment.
//
// The following member function will also be implemented:
//  - swap(file&): Swap the current FILE* with another one
//  - is_valid(): Return true if the wrapped FILE* is not null
//  - get(): Return the internal FILE* for external usage
//
////////////////////////////////////////////////////////////////////////////////////////////////////

class file
{
  FILE *file_ptr_;

public:

  
  file(file &&other) noexcept : file()
  {
  
  }

  file &operator=(file &&other)
  {
  
  }

  file(const char *filename) noexcept : file()
  {
  
  }

  ~file()
  {
  
  }

  void swap(file &other) noexcept
  {
  
  }

  bool is_valid() const
  {
  
  }

  FILE *get() const
  {
  
  }
};

void test_file() {}
```

solution

```cpp
#include <stdio.h>
#include <stdlib.h>

#include <filesystem>
#include <stdexcept>
#include <string>

////////////////////////////////////////////////////////////////////////////////////////////////////
//
// Exercise 1
//
// Implement a RAII wrapper for FILE* that will use the basic C interface in a safe manner.
//
// The file class will allow to open a text file and close it on release.
// As copying FILE* makes no sense, you'll deactivate the copy constructor and assignment
// and provide a correct move constructor and assignment.
//
// The following member function will also be implemented:
//  - swap(file&): Swap the current FILE* with another one
//  - is_valid(): Return true if the wrapped FILE* is not null
//  - get(): Return the internal FILE* for external usage
//
////////////////////////////////////////////////////////////////////////////////////////////////////

class file
{
  FILE *file_ptr_;

public:

  file() : file_ptr_(nullptr) {
  }

  file(const char *filename) noexcept : file()
  {
    //if(filename != nullptr)
      file_ptr_ = fopen(filename, "r");
  }

  file(file const&)            = delete;
  file& operator=(file const&) = delete;
  
  file(file &&other) noexcept : file()
  {
    swap(other);
    //file_ptr_ = other.file_ptr_;
    //other.file_ptr_ = nullptr;
  }

  file &operator=(file &&other)
  {
    swap(other);
    return *this;
  }

  ~file()
  {
    if(is_valid()) fclose(file_ptr_);
  }

  void swap(file &other) noexcept
  {
    std::swap(file_ptr_,other.file_ptr_);
  }

  bool is_valid() const
  {
    return file_ptr_ != nullptr;
  }

  FILE *get() const
  {
    return file_ptr_;
  }
};

/*
#include <iostream>
int main()  {
  std::ofstream data("data.txt");
  data << "abc" << std::endl;
  data << "def asdfghjkl" << std::endl;
  data.close();
}*/

void test_file()  {
  file f("file.txt");
  file g;
  g = std::move(f);
  char text[200];
  fread(&text[0],1,20,g.get());
}
```
