cmake_minimum_required(VERSION 2.8.3)
project(three-filters-to-normal)



#you can change this version
find_package(OpenCV 4.2.0 REQUIRED EXACT)
find_package(Eigen3 3.3.7 REQUIRED)


set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_BUILD_TYPE Release)
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -march=native -mfma -msse2 -msse3 -msse4")
add_definitions(-std=c++17)
add_definitions(-mfma)
add_definitions(-mno-avx512f)
add_definitions(-mno-avx512vl)
add_definitions(-DUSE_SSE_AUTO)
add_definitions(-DSSE_MATHFUN_WITH_CODE)
set(CPU_ALL_OPTIMIZATIONS "SSE;SSE2;SSE3;SSSE3;SSE4_1;SSE4_2;POPCNT;AVX;FP16;AVX2;FMA3;AVX_512F;AVX512VL;AVX512F;AVX_512VL")
#SET(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS} -mavx512f -mfma -mavx512vl -mavx512bw -mavx512dq -march=knl -march=sandybridge -march=haswell")
#message('0-0000000000000000000' ${AVX_FLAGS})

ADD_DEFINITIONS(-DEIGEN_ENABLE_AVX512=ON)



include_directories(
        ${OpenCV_INCLUDE_DIRS}
        ${Eigen3_INCLUDE_DIRS}
        ${PROJECT_NAME}
        include
        include/ThirdParty
)

add_executable(example
        example/demo.cpp
        include/ThirdParty/cvrgbd/src/depth_to_3d.cpp
        include/ThirdParty/cvrgbd/src/utils.cpp
        )
#very important!!
#otherwise in some machine,it will be built with c++11, you will find some error!!!!
set_property(TARGET example PROPERTY CXX_STANDARD 17)


target_link_libraries(example
        ${OpenCV_LIBS}
        )
