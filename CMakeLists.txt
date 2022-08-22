cmake_minimum_required (VERSION 3.10)
project(Overture)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

if("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
    add_compile_options(-fdiagnostics-color=always)
elseif("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
    add_compile_options(-fcolor-diagnostics)
endif()

set(OpenGL_GL_PREFERENCE "GLVND")

include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/../dependencies/include
    ${CMAKE_CURRENT_SOURCE_DIR}/../HPL1Engine/include
    ${CMAKE_CURRENT_SOURCE_DIR}/../OALWrapper/include
)

link_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/../dependencies/lib/linux64)

set(CMAKE_EXECUTABLE_SUFFIX .bin)

file(GLOB all_sources ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp)

add_executable(Overture ${all_sources})

set_target_properties(Overture PROPERTIES
    INSTALL_RPATH_USE_LINK_PATH false
    OUTPUT_NAME overture)

find_package(OpenGL REQUIRED)
find_package(SDL REQUIRED)
find_package(SDL_ttf REQUIRED)
find_package(SDL_image REQUIRED)
find_package(Vorbis REQUIRED)
find_package(ALUT REQUIRED)
find_package(X11 REQUIRED)

target_link_libraries(Overture HPL GLAD OALWrapper Newton angelscript Cg CgGL)
target_link_libraries(Overture OpenGL::OpenGL OpenGL::GLX OpenGL::GLU OpenAL::OpenAL ALUT::ALUT Vorbis::vorbisfile)
target_link_libraries(Overture X11::X11)
target_link_libraries(Overture ${SDL_TTF_LIBRARIES} ${SDL_IMAGE_LIBRARIES})
target_include_directories(Overture
    PUBLIC ${SDL_TTF_INCLUDE_DIRS}
    PUBLIC ${SDL_IMAGE_INCLUDE_DIRS})