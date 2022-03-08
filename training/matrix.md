# matrix

squelette to start

```cpp
#include <iostream>
#include <vector>

////////////////////////////////////////////////////////////////////////////////////////////////////
// Implement a 2D matrix class using rules of 0
////////////////////////////////////////////////////////////////////////////////////////////////////
struct matrix
{
public:
  matrix(std::size_t d0 = 0, std::size_t d1 = 0) : data_(d0 * d1), dim0_(d0), dim1_(d1){
  }

  float &operator()(std::size_t i0, std::size_t i1){
    return data_[i0 + i1 * dim0_];
  }

  float const &operator()(std::size_t i0, std::size_t i1) const{
    return data_[i0 + i1 * dim0_];
  }

  std::size_t size() const{
    return data_.size();
  }
  std::size_t size(int d) const{
    return d == 0 ? dim0_ : dim1_;
  }

private:
  std::vector<float> data_;
  std::size_t dim0_, dim1_;
};

std::ostream &operator<<(std::ostream &os, matrix const &mat)
{
  for (std::size_t i1 = 0; i1 != mat.size(1); ++i1){
    std::cout << "[ ";
    for (std::size_t i0 = 0; i0 != mat.size(0); ++i0)
      std::cout << mat(i0, i1) << " ";
    std::cout << "]\n";
  }
  return os;
}

void sample_behavior()
{
  std::cout << "Empty matrix" << std::endl;

  matrix vide;
  std::cout << vide.size() << std::endl;
  std::cout << vide.size(0) << std::endl;
  std::cout << vide.size(1) << std::endl;
  std::cout << vide << std::endl;

  std::cout << "matrix 4x5" << std::endl;

  matrix m_45(4, 5);
  std::cout << m_45.size() << std::endl;
  std::cout << m_45.size(0) << std::endl;
  std::cout << m_45.size(1) << std::endl;

  for (std::size_t i1 = 0; i1 != m_45.size(1); ++i1){
    for (std::size_t i0 = 0; i0 != m_45.size(0); ++i0)
      m_45(i0, i1) = (1 + i0) + 10 * (1 + i1);
  }

  std::cout << m_45 << std::endl;
}
```
