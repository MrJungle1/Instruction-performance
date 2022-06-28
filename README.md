# Instruction-performance
This is a benchmark for testing instruction throughput, GFlops, latency, supports arm, x86.

## BUILD
This benchmark only support [Cmake](https://cmake.org/) build system and require Cmake version upper than 3.15.2, you can compiler the benchmark fellow the step:

* clone or download the project
* choose a test plantform, the current default processor is aarch64. If the processor is other architecture, only need to modify CMakeLists.txt. 
* if the processor architecture is armv7 `<set(MGEPEAK_ARCH "armv7")>`
* if the processor architecture is x86 `<set(MGEPEAK_ARCH "x86")>`
* A gcc or clang compier should find by camke through PATH env


  ```bash
  mkdir -p build && cd build
  cmake .. 
  make
   ```
* after build, the executable file megpeak is stored in build drector

### Run
if you compiler the project and get the megpeak.
    ```bash
    ./megpeak
     ```
