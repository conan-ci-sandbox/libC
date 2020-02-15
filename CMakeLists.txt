cmake_minimum_required(VERSION 2.8.12)
project(conan-ci-cd-training)

include(${CMAKE_CURRENT_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup(TARGETS)

set(SOURCES src/libC.cpp)
file(GLOB HEADERS include/libC/*)

add_library(libC ${SOURCES} ${HEADERS})

target_link_libraries(libC PRIVATE CONAN_PKG::libA)

target_include_directories(
    libC PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)

install(TARGETS libC
        RUNTIME DESTINATION ${CMAKE_INSTALL_PREFIX}/bin
        LIBRARY DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
        ARCHIVE DESTINATION ${CMAKE_INSTALL_PREFIX}/lib)
install(FILES ${HEADERS}
        DESTINATION ${CMAKE_INSTALL_PREFIX}/include/libC
        PERMISSIONS OWNER_WRITE OWNER_READ GROUP_READ WORLD_READ)
