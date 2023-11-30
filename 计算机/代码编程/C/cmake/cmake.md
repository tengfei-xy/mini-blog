# cmake

## 目录

-   [CMakeLists](#CMakeLists)

# CMakeLists

```text
cmake_minimum_required(VERSION 3.26)

project(p2cEnode C)

set(CMAKE_C_STANDARD 11)

find_package(OpenSSL REQUIRED)
include_directories(${OPENSSL_INCLUDE_DIR})
add_executable(p2cEnode main.c bit.c strings.c system.c time.c
        bogus.c)
target_link_libraries(p2cEnode ${OPENSSL_LIBRARIES})

```
