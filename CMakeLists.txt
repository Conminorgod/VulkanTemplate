cmake_minimum_required(VERSION 3.10)

project(VulkanGameEngine)

find_package(Vulkan REQUIRED)

find_package(PkgConfig REQUIRED)
pkg_check_modules(GLFW REQUIRED glfw3)

include_directories(
    ${Vulkan_INCLUDE_DIRS}
    ${GLFW_INCLUDE_DIRS}
)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -std=c++17")
set(source_dir "${PROJECT_SOURCE_DIR}/src/")

function(compile_shader TARGET_NAME SHADER_DIR OUTPUT_DIR)
    find_program(GLSLC_EXECUTABLE glslc HINTS
        ${Vulkan_GLSLC_EXECUTABLE}
        $ENV{VULKAN_SDK}/bin
    )

    if(NOT GLSLC_EXECUTABLE)
        message(FATAL_ERROR "glslc not found. Please ensure Vulkan SDK is installed and GLSLC is in your PATH.")
    endif()

    file(MAKE_DIRECTORY ${OUTPUT_DIR})

    file(GLOB SHADER_FILES
        "${SHADER_DIR}/*.vert"
        "${SHADER_DIR}/*.frag"
    )

    foreach(SHADER_FILE ${SHADER_FILES})
        get_filename_component(SHADER_NAME ${SHADER_FILE} NAME)
        get_filename_component(SHADER_EXT ${SHADER_FILE} EXT)

        if (SHADER_EXT STREQUAL ".vert")
            set(SPIRV_FILE "${OUTPUT_DIR}/vert.spv")
        elseif (SHADER_EXT STREQUAL ".frag")
            set(SPIRV_FILE "${OUTPUT_DIR}/frag.spv")
        else()
            message(FATAL_ERROR "Unsupported shader type for file: ${SHADER_NAME}")
        endif()

        add_custom_command(
            OUTPUT ${SPIRV_FILE}
            COMMAND ${GLSLC_EXECUTABLE} -o ${SPIRV_FILE} ${SHADER_FILE}
            DEPENDS ${SHADER_FILE}
            COMMENT "Compiling shader ${SHADER_NAME} to SPIR-V"
            VERBATIM
        )

        list(APPEND SPIRV_FILES ${SPIRV_FILE})
    endforeach()

    add_custom_target(${TARGET_NAME} ALL DEPENDS ${SPIRV_FILES})
endfunction()

set(SHADER_DIR "${source_dir}/shaders")
set(SPIRV_OUTPUT_DIR "${CMAKE_BINARY_DIR}/shaders")
compile_shader(CompileShaders ${SHADER_DIR} ${SPIRV_OUTPUT_DIR})

add_executable(VulkanGameEngine ./src/main.cpp)
target_link_libraries(VulkanGameEngine 
    ${Vulkan_LIBRARIES}
    ${GLFW_LIBRARIES}
)

add_dependencies(VulkanGameEngine CompileShaders)