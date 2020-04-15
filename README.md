# How to implement a basic ray tracing renderer


# Table of Contents

1.  [How to implement a basic ray tracing renderer](#org51b3e67)
    1.  [Preparation](#org6a233ef)

## Preparation

As a starter, let's simply paint some color first, and ouput it to a pixmap file. We use [PPM](https://en.wikipedia.org/wiki/Netpbm). It is a very simple format for pixmap.

```C++
#include <cassert>
#include <cmath>
#include <vector>
template <size_t DIM , typename T> struct vec {
  vec() { for (size_t i=DIM; i--; data_[i] = T()); }
  T& operator[](const size_t i) { assert(i<DIM); return data_[i]; }
  const T& operator[](const size_t i) const { assert(i<DIM); return data_[i]; }
private:
  T data_[DIM];
};

typedef vec<2, float> Vec2f;
typedef vec<3, float> Vec3f;

template <typename T> struct vec<3, T> {
  vec() : x(T()), y(T()), z(T()) {}
  vec(T X, T Y, T Z) : x(X), y(Y), z(Z) {}
  T& operator[](const size_t i) { assert(i<3); return i<=0 ? x : (1==i ? y : z); }
  const T& operator[](const size_t i) const { assert(i<3); return i<=0 ? x : (1==i ? y : z); }
  T x,y,z;
};
```

```C++
#include <cassert>
#include <cmath>
#include <vector>
template <size_t DIM , typename T> struct vec {
  vec() { for (size_t i=DIM; i--; data_[i] = T()); }
  T& operator[](const size_t i) { assert(i<DIM); return data_[i]; }
  const T& operator[](const size_t i) const { assert(i<DIM); return data_[i]; }
private:
  T data_[DIM];
};

typedef vec<2, float> Vec2f;
typedef vec<3, float> Vec3f;

template <typename T> struct vec<3, T> {
  vec() : x(T()), y(T()), z(T()) {}
  vec(T X, T Y, T Z) : x(X), y(Y), z(Z) {}
  T& operator[](const size_t i) { assert(i<3); return i<=0 ? x : (1==i ? y : z); }
  const T& operator[](const size_t i) const { assert(i<3); return i<=0 ? x : (1==i ? y : z); }
  T x,y,z;
};

#include <iostream>
#include <cmath>
#include <vector>
#include <fstream>

void output(framebuffer) {
  std::ofstream ofs; // save the framebuffer to file
  ofs.open("./out.ppm");
  ofs << "P6\n" << width << " " << height << "\n255\n";

  for (size_t i = 0; i < height*width; ++i) {
    for (size_t j = 0; j < 3; ++j) {
      ofs << (char)(255 * std::max(0.f, std::min(1.f, framebuffer[i][j])));
    }
  }
  ofs.close();
}

void render() {
  const int width = 200;
  const int height = 100;

  std::vector<Vec3f> framebuffer(width*height);

  for (size_t j = 0; j < height; j++) {
    for (size_t i = 0; i < width; i++) {
      framebuffer[i+j*width] = Vec3f(j/float(height), i/float(width), 0);
    }
  }

  output(framebuffer);
}

int main() {
  render();
  return 0;
}
```
