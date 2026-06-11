cmake_minimum_required(VERSION 3.21)
project(hello_window)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# ── GLAD (from LearnOpenGL project) ──────────────────────────────────
set(GLAD_SRC   "${CMAKE_SOURCE_DIR}/../../../src/glad.c")
set(GLAD_INC   "${CMAKE_SOURCE_DIR}/../../../includes")

# ── GLFW (MinGW pre-built binaries) ──────────────────────────────────
set(GLFW_DIR   "D:/CODE/QT/QML/QML/Tools/openGL/glfw-3.4.bin.WIN64")
set(GLFW_INC   "${GLFW_DIR}/include")
set(GLFW_LIB   "${GLFW_DIR}/lib-mingw-w64")

# ── Executable ───────────────────────────────────────────────────────
add_executable(hello_window
    # WIN32
    hello_window_clear.cpp
    ${GLAD_SRC}
)

target_include_directories(hello_window PRIVATE
    ${GLAD_INC}
    ${GLFW_INC}
)

target_link_libraries(hello_window
    "${GLFW_LIB}/libglfw3dll.a"
    opengl32
    gdi32
)

# ── Copy GLFW DLL to output directory after build ────────────────────
add_custom_command(TARGET hello_window POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
        "${GLFW_LIB}/glfw3.dll"
        $<TARGET_FILE_DIR:hello_window>
    COMMENT "Copying glfw3.dll to output directory"
)
