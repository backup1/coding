# bad polymorphism matrix

example

```
#include <chrono>
#include <iostream>
#include <memory>
#include <vector>

////////////////////////////////////////////////////////////////////////////////////////////////////
// Virtual function code are costly. If you're not careful enough, it'll dominate the actual cost
// of the function you call. Therefore, the virtual functions must be big enough to stay relevant.
//
// This sample code benchmark this issue on a simple case of two matrix types overloading the
// very fast operator().
////////////////////////////////////////////////////////////////////////////////////////////////////
struct base_matrix
{
  virtual double operator()(int r, int c) const = 0;
  virtual double &operator()(int r, int c) = 0;

  base_matrix(int n) : size_(n)
  {
  }
  virtual ~base_matrix()
  {
  }

  int height() const
  {
    return size_;
  }
  int width() const
  {
    return size_;
  }

private:
  int size_;
};

struct full_matrix : public base_matrix
{
  full_matrix(int n) : base_matrix(n), storage_(n * n)
  {
  }

  using base_matrix::height;
  using base_matrix::width;

  double operator()(int r, int c) const override
  {
    return storage_[c + width() * r];
  }

  double &operator()(int r, int c) override
  {
    return storage_[c + width() * r];
  }

private:
  std::vector<double> storage_;
};

struct diagonal_matrix : public base_matrix
{
  diagonal_matrix(int n) : base_matrix(n), storage_(n), zero_{0.0}
  {
  }

  virtual double operator()(int r, int c) const
  {
    return (r == c) ? storage_[r] : 0.;
  }

  virtual double &operator()(int r, int c)
  {
    return (r == c) ? storage_[r] : (zero_ = 0.);
  }

  using base_matrix::height;
  using base_matrix::width;

private:
  std::vector<double> storage_;
  double zero_;
};

double trace(base_matrix const &m)
{
  double result = {};

  auto d = std::min(m.height(), m.width());

  for (int i = 0; i < d; ++i)
    result += m(i, i);

  return result;
}

double res;

void sample_behavior()
{
  std::size_t n = 500;
  std::size_t repetition = 10000;

  {
    auto ptr = std::make_unique<full_matrix>(n);

    auto start = std::chrono::steady_clock::now();
    for (std::size_t i = 0; i < repetition; ++i)
      res = trace(*ptr);
    auto stop = std::chrono::steady_clock::now();

    auto timing = stop - start;

    std::cout << "Dynamic: " << (std::chrono::duration<double, std::nano>(timing).count()) / repetition << "\n";
  }

  {
    full_matrix mat(n);

    auto start = std::chrono::steady_clock::now();
    for (std::size_t i = 0; i < repetition; ++i)
      res = trace(mat);
    auto stop = std::chrono::steady_clock::now();

    auto timing = stop - start;

    std::cout << "Static: " << (std::chrono::duration<double, std::nano>(timing).count()) / repetition << "\n";
  }
}


```
