cmake_minimum_required(VERSION 3.14)
project(my_project)

set(CMAKE_CXX_STANDARD 14)

include(FetchContent)
FetchContent_Declare(fmt
  GIT_REPOSITORY https://github.com/fmtlib/fmt.git
  GIT_TAG master
)
FetchContent_MakeAvailable(fmt)

FetchContent_Declare(
  hermes
  URL https://github.com/facebook/hermes/archive/d63feeb46d26fe0ca7e789fc793f409e5158b27f.zip
)
FetchContent_MakeAvailable(hermes)

FetchContent_Declare(
  googletest
  URL https://github.com/google/googletest/archive/609281088cfefc76f9d0ce82e1ff6c30cc3591e5.zip
)
# For Windows: Prevent overriding the parent project's compiler/linker settings
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)


# set(GTEST_ROOT "/Users/yinglao/googletest")
# set(GTEST_LIBRARY /Users/yinglao/test/googletest/build/lib/libgtest.a)
#set(GTEST_INCLUDE_DIR /Users/yinglao/test/googletest/googletest/include)
# set(GTEST_MAIN_LIBRARY /Users/yinglao/test/googletest/build/lib/libgtest_main.a)
# find_package(GTest REQUIRED)

if(gtest_main)
  message("libgtest_main found")
endif()

enable_testing()

add_executable(
  hello_test
  mytest.cpp
)

set(TORCH_DIR "/Users/yinglao/pytorch-install")
target_include_directories(hello_test PUBLIC "${TORCH_DIR}/include/" "../src/")

target_link_libraries(
  hello_test
  # GTest::gtest_main
  gtest_main
  fmt
  libhermes
  jsi
  ${TORCH_DIR}/lib/libXNNPACK.a
  ${TORCH_DIR}/lib/libpytorch_qnnpack.a
  ${TORCH_DIR}/lib/libc10.dylib
  ${TORCH_DIR}/lib/libshm.dylib
  ${TORCH_DIR}/lib/libclog.a
  ${TORCH_DIR}/lib/libsleef.a

  ${TORCH_DIR}/lib/libcpuinfo.a
  ${TORCH_DIR}/lib/libtorch.dylib
  ${TORCH_DIR}/lib/libkineto.a
  ${TORCH_DIR}/lib/libtorch_cpu.dylib

  ${TORCH_DIR}/lib/libnnpack.a
  ${TORCH_DIR}/lib/libtorch_global_deps.dylib
  ${TORCH_DIR}/lib/libprotobuf-lite.a

  # ${TORCH_DIR}/lib/libtorch_python.dylib
  ${TORCH_DIR}/lib/libprotobuf.a
  ${TORCH_DIR}/lib/libprotoc.a




)

get_target_property(HELLO_TEST_LIBRARIES hello_test LINK_LIBRARIES)

include(CMakePrintHelpers)
cmake_print_variables(HELLO_TEST_LIBRARIES)

include(GoogleTest)
gtest_discover_tests(hello_test)
