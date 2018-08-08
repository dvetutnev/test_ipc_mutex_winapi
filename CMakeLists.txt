# Test IPC mutex WinAPI
# Dmitriy Vetutnev, 2018


cmake_minimun_required(VERSION 3.0)
project(test_ipc_mutex_winapi CXX)


if(NOT WIN32)
    message(FATAL_ERROR "This project build only Windows!")
endif()


option(USE_CONAN "Use Conan package manager" ON)
if(USE_CONAN)

    include(conan.cmake)
    conan_check(REQUIRED)

    conan_add_remote(
        NAME kysa
        URL https://conan.kysa.me
    )

    conan_cmake_run(
    )

endif()


find_package(Boost REQUIRED)

add_definitions(-DBOOST_ERROR_CODE_HEADER_ONLY)

enable_testing()


add_excutable(simple simple.cpp)
target_link_libraries(simple
    Boost::boost
    Boost::disable_autolinking
)
add_test(
    NAME simple
    COMMAND simple
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/bin
)
set_property(TEST simple PROPERTY TIMEOUT 100)

