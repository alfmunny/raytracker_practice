- [How to implement a basic ray tracing renderer](#sec-1)
  - [Preparation](#sec-1-1)

# How to implement a basic ray tracing renderer<a id="sec-1"></a>


## Preparation<a id="sec-1-1"></a>

As a starter, let's simply paint some color first, and ouput it to a pixmap file. We use [PPM](https://en.wikipedia.org/wiki/Netpbm). It is a very simple format for pixmap.

```C++
<<geometry>>

#include <iostream>
#include <cmath>
#include <vector>
#include <fstream>

void output(const int width, const int height, std::vector<Vec3f> framebuffer) {
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
  const int width = 1024;
  const int height = 768;

  std::vector<Vec3f> framebuffer(width*height);

  for (size_t j = 0; j < height; j++) {
    for (size_t i = 0; i < width; i++) {
      framebuffer[i+j*width] = Vec3f(j/float(height), i/float(width), 0);
    }
  }

  output(width, height, framebuffer);
}

int main() {
  render();
  return 0;
}
```
