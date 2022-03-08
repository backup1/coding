# polymorphism matrix

squelette to start

```cpp
#include <chrono>
#include <iostream>
#include <memory>
#include <vector>
#include <algorithm>
#include <numeric>
#include <vector>

//==================================================================================================
//  Exercise 3
//
//  In this exercise, we are building a class hierarchy for matrices:
//    - Dense matrices
//    - Diagonale matrices
//
//  for which we'll design an interface to compute their trace (ie sum of their diagonal elements).
//  This implementation should have better time performance than the 03 sample
//==================================================================================================

//==================================================================================================
// matrix_interface provides the storage which is initializeed with 0, 1, ..., n by init(). 
// It represents the dense, row-major for the dense matrix and the diagonal for the diagonal_matrix
// class.
//==================================================================================================
class matrix_interface
{
protected:
  std::size_t width_, height_;
  std::vector<double> data_;

  void init(){
    std::iota(this->data_.begin(), this->data_.end(), 0.);
  }

public:
  matrix_interface(std::size_t width, std::size_t height, std::size_t size)
      : width_(width), height_(height), data_(size, 0){
    this->init();
  }

  matrix_interface() : width_(0), height_(0), data_(){
  }

  virtual ~matrix_interface() {}
  virtual double trace() const = 0;

  inline std::size_t width() const{
    return this->width_;
  }

  inline std::size_t height() const{
    return this->height_;
  }

  inline std::size_t size() const{
    return this->data_.size();
  }

  inline void throw_out_of_range(std::size_t row, std::size_t col){
    throw std::out_of_range("out of range " + std::to_string(row) + ", " + std::to_string(col));
  }
};

////////////////////////////////////////////////////////////////////////////////////////////////////
//  Implements the dense_matrix class, an implementation of matrix_interface
////////////////////////////////////////////////////////////////////////////////////////////////////
class dense_matrix : public matrix_interface {
public:
  dense_matrix() : matrix_interface(){
  }

  dense_matrix(std::size_t w, std::size_t h) : matrix_interface(w, h, w * h){
  }
};

////////////////////////////////////////////////////////////////////////////////////////////////////
//  Implements the diagonal_matrix class, an implementation of matrix_interface
////////////////////////////////////////////////////////////////////////////////////////////////////
class diagonal_matrix : public matrix_interface {
public:
  diagonal_matrix() : matrix_interface(){
  }

  diagonal_matrix(std::size_t w, std::size_t h) : matrix_interface(w, h, std::min(w, h)){
  }
};

double trace(matrix_interface const &m){
  return m.trace();
}

double res;

void bench_tests(int c){
  std::size_t n = 100;
  std::size_t repetition = 1000;

  {
    std::unique_ptr<matrix_interface> ptr;

    if(c == 1) 
      ptr = std::make_unique<dense_matrix>(n,n);
    
    auto start = std::chrono::steady_clock::now();
    for (std::size_t i = 0; i < repetition; ++i)
      res = trace(*ptr);
    auto stop = std::chrono::steady_clock::now();

    auto timing = stop - start;

    std::cout << "Dynamic: " << (std::chrono::duration<double, std::nano>(timing).count()) / repetition << "\n";
  }

  {
    dense_matrix mat(n,n);

    auto start = std::chrono::steady_clock::now();
    for (std::size_t i = 0; i < repetition; ++i)
      res = trace(mat);
    auto stop = std::chrono::steady_clock::now();

    auto timing = stop - start;

    std::cout << "Static: " << (std::chrono::duration<double, std::nano>(timing).count()) / repetition << "\n";
  }
}
```
