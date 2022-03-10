# matrix sum

squelette to start

```cpp
#include <chrono>
#include <iostream>
#include <memory>
#include <vector>

////////////////////////////////////////////////////////////////////////////////////////////////////
//  Let's compare three strategies for computing sum of a 2D array elements:
//
//    - sum along lines (inner_sum)
//    - sum along columns, column by column (outer_sum)
//    - sum along colums, line by line (outer_sum_2)
//  Try to explain the measure we obtain.
////////////////////////////////////////////////////////////////////////////////////////////////////
struct matrix
{
public:
  matrix(std::size_t d0 = 0, std::size_t d1 = 0) : data_(d0 * d1), dim0_(d0), dim1_(d1)
  {
  }

  float &operator()(std::size_t i0, std::size_t i1)
  {
    return data_[i0 + i1 * dim0_];
  }

  float operator()(std::size_t i0, std::size_t i1) const
  {
    return data_[i0 + i1 * dim0_];
  }

  std::size_t size() const
  {
    return data_.size();
  }
  std::size_t size(int d) const
  {
    return d == 0 ? dim0_ : dim1_;
  }

private:
  std::vector<float> data_;
  std::size_t dim0_, dim1_;
};

matrix inner_sum(matrix const &mat)
{
}

matrix outer_sum(matrix const &mat)
{
}

matrix outer_sum_2(matrix const &mat)
// faster than inner_sum because the compiler can parallelise the sum which are independant each other, so we can do 4 by 4 internally
// inner product is addss : add single single, here it is addps : add pack single
{
}

matrix res_inner, res_outer, res_outer_2;

void sample_behavior()
{
  int n = 500;
  int r = 1000;

  matrix mat(n, n);

  for (std::size_t i = 0; i != mat.size(1); ++i)
  {
    for (std::size_t j = 0; j != mat.size(0); ++j)
      mat(j, i) = (j + 1) + 1000 * (i + 1);
  }

  {
    auto start = std::chrono::steady_clock::now();
    for (int i = 0; i < r; ++i)
      res_inner = inner_sum(mat);
    auto stop = std::chrono::steady_clock::now();

    auto timing = stop - start;

    std::cout << "inner_sum: " << std::chrono::duration<double, std::micro>(timing).count() / r << "\n";
  }

  {
    auto start = std::chrono::steady_clock::now();
    for (int i = 0; i < r; ++i)
      res_outer = outer_sum(mat);
    auto stop = std::chrono::steady_clock::now();

    auto timing = stop - start;

    std::cout << "outer_sum: " << std::chrono::duration<double, std::micro>(timing).count() / r << "\n";
  }

  {
    auto start = std::chrono::steady_clock::now();
    for (int i = 0; i < r; ++i)
      res_outer_2 = outer_sum_2(mat);
    auto stop = std::chrono::steady_clock::now();

    auto timing = stop - start;

    std::cout << "outer_sum 2: " << std::chrono::duration<double, std::micro>(timing).count() / r << "\n";
  }
}
```

solution

```
#include <chrono>
#include <iostream>
#include <memory>
#include <vector>

////////////////////////////////////////////////////////////////////////////////////////////////////
//  Let's compare three strategies for computing sum of a 2D array elements:
//
//    - sum along lines (inner_sum)
//    - sum along columns, column by column (outer_sum)
//    - sum along colums, line by line (outer_sum_2)
//  Try to explain the measure we obtain.
////////////////////////////////////////////////////////////////////////////////////////////////////
struct matrix
{
public:
  matrix(std::size_t d0 = 0, std::size_t d1 = 0) : data_(d0 * d1), dim0_(d0), dim1_(d1)
  {
  }

  float &operator()(std::size_t i0, std::size_t i1)
  {
    return data_[i0 + i1 * dim0_];
  }

  float operator()(std::size_t i0, std::size_t i1) const
  {
    return data_[i0 + i1 * dim0_];
  }

  std::size_t size() const
  {
    return data_.size();
  }
  std::size_t size(int d) const
  {
    return d == 0 ? dim0_ : dim1_;
  }

private:
  std::vector<float> data_;
  std::size_t dim0_, dim1_;
};

matrix inner_sum(matrix const &mat)
{
  matrix ret(1,mat.size(1));
  for(int col = 0; col < mat.size(1); ++col){
	for(int row = 0; row < mat.size(0); ++row) ret(0,col) += mat(row,col);
  }
  return ret;
}

matrix outer_sum(matrix const &mat)
{
  matrix ret(mat.size(0),1);
  for(int row = 0; row < mat.size(0); ++row){
	for(int col = 0; col < mat.size(1); ++col) ret(row,0) += mat(row,col);
  }
  return ret;
}

matrix outer_sum_2(matrix const &mat)
// faster than inner_sum because the compiler can parallelise the sum which are independant each other, so we can do 4 by 4 internally
// inner product is addss : add single single, here it is addps : add pack single
{
  matrix ret(mat.size(0),1);
  for(int col = 0; col < mat.size(1); ++col){
	for(int row = 0; row < mat.size(0); ++row) ret(row,0) += mat(row,col);
  }
  return ret;
}

matrix res_inner, res_outer, res_outer_2;

void sample_behavior()
{
  int n = 500;
  int r = 1000;

  matrix mat(n, n);

  for (std::size_t i = 0; i != mat.size(1); ++i)
  {
    for (std::size_t j = 0; j != mat.size(0); ++j)
      mat(j, i) = (j + 1) + 1000 * (i + 1);
  }

  {
    auto start = std::chrono::steady_clock::now();
    for (int i = 0; i < r; ++i)
      res_inner = inner_sum(mat);
    auto stop = std::chrono::steady_clock::now();

    auto timing = stop - start;

    std::cout << "inner_sum: " << std::chrono::duration<double, std::micro>(timing).count() / r << "\n";
  }

  {
    auto start = std::chrono::steady_clock::now();
    for (int i = 0; i < r; ++i)
      res_outer = outer_sum(mat);
    auto stop = std::chrono::steady_clock::now();

    auto timing = stop - start;

    std::cout << "outer_sum: " << std::chrono::duration<double, std::micro>(timing).count() / r << "\n";
  }

  {
    auto start = std::chrono::steady_clock::now();
    for (int i = 0; i < r; ++i)
      res_outer_2 = outer_sum_2(mat);
    auto stop = std::chrono::steady_clock::now();

    auto timing = stop - start;

    std::cout << "outer_sum 2: " << std::chrono::duration<double, std::micro>(timing).count() / r << "\n";
  }
}
```

```
/**
-Ofast -march=native ==> attention, this change the order of the calculation
inner_sum: 36.4055
outer_sum: 234.741
outer_sum 2: 35.8821
**/
// -O3 -DNDEBUG
// -march=skylake-avx512 : par 512 bits / for clang
// -marvx512f : same, but for gcc / g++
// >_ Arguments : --timeline
// option _mm_prefetch passe du soft au hardware / compilo aujourd'hui
// info CPU : ~$ lscpu
// Valgrind to investigate Cahcegrind : ~$ valgrind --tool=cachegrind ./a.out
// reformat the output : ~$cg_annotate cachegrind.out.12571 --auto=yes > out.txt
// reformat the output : ~$cg_annotate cachegrind.out.12571 source.cpp > out.txt
// pas de if dans for loop, si possible
```
