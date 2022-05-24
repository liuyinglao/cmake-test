cmake_minimum_required(VERSION 3.13)
project(pytorch_live_cxx)

set(CMAKE_CXX_STANDARD 14)

include(FetchContent)

FetchContent_Declare(
  hermes
  URL https://github.com/facebook/hermes/archive/d63feeb46d26fe0ca7e789fc793f409e5158b27f.zip
)

FetchContent_Declare(
  googletest
  URL https://github.com/google/googletest/archive/609281088cfefc76f9d0ce82e1ff6c30cc3591e5.zip
)

enable_testing()

add_executable(
  hello_test
  example.cpp
)

target_link_libraries(
  hello_test
  gtest_main
  libhermes
  jsi
)

if(gtest_main)
  message("gtest_main found")
endif()

FetchContent_MakeAvailable(hermes)
FetchContent_MakeAvailable(googletest)

include(GoogleTest)
gtest_discover_tests(hello_test)
