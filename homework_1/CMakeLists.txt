cmake_minimum_required(VERSION 3.10)
project(homework_1)


set(PATCH_VERSION "2" CACHE INTERNAL "Patch version")
set(PROJECT_VERSION 0.1.${PATCH_VERSION})

project(homework_1 VERSION ${PROJECT_VESRION})

find_package(Boost COMPONENTS unit_test_framework REQUIRED)

configure_file(version.h.in version.h)

add_executable(homework_1_cli main.cpp)
add_library(homework_1 lib.cpp)
add_executable(test_version test_version.cpp)

set_target_properties(homework_1_cli homework_1 test_version PROPERTIES
        CXX_STANDARD 14
        CXX_STANDARD_REQUIRED ON
)
target_include_directories(homework_1
        PRIVATE "${CMAKE_BINARY_DIR}"
)

set_target_properties(test_version PROPERTIES
        COMPILE_DEFINITIONS BOOST_TEST_DYN_LINK
        INCLUDE_DIRECTORIES ${Boost_INCLUDE_DIR}
)

target_link_libraries(homework_1_cli PRIVATE
        homework_1
)
target_link_libraries(test_version
        ${Boost_LIBRARIES}
        homework_1
)

set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")