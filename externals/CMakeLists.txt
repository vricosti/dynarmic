# For libraries that already come with a CMakeLists file,
# simply add the directory to that file as a subdirectory
# to have CMake automatically recognize them.

# catch

add_library(catch INTERFACE)
target_include_directories(catch INTERFACE $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/catch>)

# fmt

if (NOT DYNARMIC_NO_BUNDLED_FMT)
    # fmtlib formatting library
    add_subdirectory(fmt)
endif()

# mp

add_library(mp INTERFACE)
target_include_directories(mp INTERFACE $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/mp/include>)

# robin-map

add_library(robin_map INTERFACE)
add_library(tsl::robin_map ALIAS robin_map)
target_include_directories(robin_map SYSTEM INTERFACE "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/robin-map/include>")

# vixl

if (ARCHITECTURE STREQUAL "arm64")
    add_subdirectory(vixl)
endif()

# xbyak

if (NOT TARGET xbyak)
    if (ARCHITECTURE STREQUAL "x86" OR ARCHITECTURE STREQUAL "x86_64")
        add_library(xbyak INTERFACE)
        target_include_directories(xbyak SYSTEM INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/xbyak/xbyak)
        target_compile_definitions(xbyak INTERFACE XBYAK_NO_OP_NAMES)
    endif()
endif()

if (NOT TARGET xbyak_aarch64)
    if(ARCHITECTURE_ARM64)
        add_library(xbyak_aarch64 INTERFACE)
        target_include_directories(xbyak_aarch64 SYSTEM INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/xbyak_aarch64/xbyak_aarch64)
        target_compile_definitions(xbyak_aarch64 INTERFACE XBYAK_NO_OP_NAMES)
    endif()
endif()


