# copy constructor vs move constructor

Copy constructor

```cpp
#include <bits/stdc++.h>
using namespace std;

class MyClass{
private:
  static int sCounter;
  int *mMember{&sCounter};
public:
  MyClass(){
    ++(*mMember);
    cout << "Construction " << getValue() << endl;
  }
  ~MyClass(){
    --(*mMember);
    mMember = nullptr;
    cout << "Destruction " << sCounter << endl;
  }
  MyClass(const MyClass& rhs) : mMember{rhs.mMember} {
    ++(*mMember);
    cout << "Copy " << getValue() << endl;
  }
  int getValue() const{
    return *mMember;
  }
};

int MyClass::sCounter{0};

MyClass CopyMyClass(MyClass parameter){
  return parameter;
}

int main(){
  auto object1 = MyClass();
  {
    auto object2 = MyClass();
  }
  auto object3 = MyClass();
  auto object4 = CopyMyClass(object3);
  return 0;
}
```

```bash
$ ./a.exe
Construction 1
Construction 2
Destruction 1
Construction 2
Copy 3
Copy 4
Destruction 3
Destruction 2
Destruction 1
Destruction 0
```

Move constructor

```cpp
#include <bits/stdc++.h>
using namespace std;

class MyClass{
private:
  static int sCounter;
  int *mMember{&sCounter};
public:
  MyClass(){
    ++(*mMember);
    cout << "Construction " << getValue() << endl;
  }
  ~MyClass(){
    if(mMember){
      --(*mMember);
      mMember = nullptr;
      cout << "Destruction " << sCounter << endl;
    }
    else{
      cout << "Destruction of a moved-from instance" << endl;
    }
  }
  MyClass(const MyClass& rhs) : mMember{rhs.mMember} {
    ++(*mMember);
    cout << "Copy " << getValue() << endl;
  }
  MyClass(MyClass&& rhs) : mMember{rhs.mMember} {
    cout << hex << showbase;
    cout << "Move " << &rhs << " to " << this << endl;
    cout << noshowbase << dec;
    rhs.mMember = nullptr;
  }
  int getValue() const{
    return *mMember;
  }
};

int MyClass::sCounter{0};

MyClass CopyMyClass(MyClass parameter){
  return parameter;
}

int main(){
  auto object1 = MyClass();
  {
    auto object2 = MyClass();
  }
  auto object3 = MyClass();
  auto object4 = CopyMyClass(object3);
  return 0;
}
```

```bash
$ ./a.exe
Construction 1
Construction 2
Destruction 1
Construction 2
Copy 3
Move 0xffffcc28 to 0xffffcc10
Destruction of a moved-from instance
Destruction 2
Destruction 1
Destruction 0
```

Compare copy constructor against move constructor

(seems that emplace\_back is not so useful)

```cpp
#include <bits/stdc++.h>
using namespace std;
using namespace chrono;
using namespace literals;

class MyClass{
private:
  vector<string> mString{
    "This is a pretty long string that"
    " must be copy constructed into"
    " copyConstructed!"s
  };
  int mValue{ 1 };
public:
  MyClass() = default;
  MyClass(const MyClass& rhs) = default;
  MyClass(MyClass&& rhs) = default;
  int getValue() const{
    return mValue;
  }
};

int main(){
  using MyVector = vector<MyClass>;
  constexpr unsigned int ITERATIONS{1000000U};
  int value{0};

  MyVector copyConstructed;
  auto copyStartTime = high_resolution_clock::now();
  for(unsigned int i = 0; i < ITERATIONS; ++i){
    MyClass myClass;
    copyConstructed.push_back(myClass);
    value = myClass.getValue();
  }
  auto copyEndTime = high_resolution_clock::now();
  auto copyDuration = duration_cast<milliseconds>(copyEndTime-copyStartTime);
  cout << "Copy lasted " << copyDuration.count() << "ms" << endl;

  MyVector copyConstructed2;
  auto copyStartTime2 = high_resolution_clock::now();
  for(unsigned int i = 0; i < ITERATIONS; ++i){
    MyClass myClass;
    copyConstructed2.emplace_back(myClass);
    value = myClass.getValue();
  }
  auto copyEndTime2 = high_resolution_clock::now();
  auto copyDuration2 = duration_cast<milliseconds>(copyEndTime2-copyStartTime2);
  cout << "Copy (emplace_back) lasted " << copyDuration2.count() << "ms" << endl;

  MyVector moveConstructed;
  auto moveStartTime = high_resolution_clock::now();
  for(unsigned int i = 0; i < ITERATIONS; ++i){
    MyClass myClass;
    moveConstructed.push_back(move(myClass));
    value = myClass.getValue();
  }
  auto moveEndTime = high_resolution_clock::now();
  auto moveDuration = duration_cast<milliseconds>(moveEndTime-moveStartTime);
  cout << "Move lasted " << moveDuration.count() << "ms" << endl;

  MyVector moveConstructed2;
  auto moveStartTime2 = high_resolution_clock::now();
  for(unsigned int i = 0; i < ITERATIONS; ++i){
    MyClass myClass;
    moveConstructed2.emplace_back(move(myClass));
    value = myClass.getValue();
  }
  auto moveEndTime2 = high_resolution_clock::now();
  auto moveDuration2 = duration_cast<milliseconds>(moveEndTime2-moveStartTime2);
  cout << "Move (emplace_back) lasted " << moveDuration2.count() << "ms" << endl;

  return 0;
}
```

```bash
$ ./a.exe
Copy lasted 569ms
Copy (emplace_back) lasted 572ms
Move lasted 403ms
Move (emplace_back) lasted 423ms
```
