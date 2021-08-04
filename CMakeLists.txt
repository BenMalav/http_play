# Referenced - https://dev.to/younup/cmake-on-stm32-the-beginning-3766
# https://pabloariasal.github.io/2018/02/19/its-time-to-do-cmake-right/
# https://interrupt.memfault.com/blog/best-and-worst-gcc-clang-compiler-flags
# https://hsf-training.github.io/hsf-training-cmake-webpage/aio/index.html

cmake_minimum_required(VERSION 3.20)
project(http_play)

set(ENV{PKG_CONFIG_PATH} "/opt/homebrew/opt/openssl@1.1/lib/pkgconfig")

set(CMAKE_CXX_STANDARD 11)

set(PROJECT_FILES
        main.cpp)

set(EXECUTABLE ${PROJECT_NAME}.out)

add_executable(${EXECUTABLE} ${PROJECT_FILES})

find_package(PkgConfig REQUIRED)
pkg_check_modules(OPENSSL REQUIRED IMPORTED_TARGET openssl)

target_link_libraries(${EXECUTABLE} PRIVATE
        PkgConfig::OPENSSL
        )

target_include_directories(${EXECUTABLE} PRIVATE
        PkgConfig::OPENSSL
        )

# Print executable size
add_custom_command(TARGET ${EXECUTABLE}
        POST_BUILD
        COMMAND size ${EXECUTABLE})
