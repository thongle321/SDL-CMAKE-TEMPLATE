cmake_minimum_required(VERSION 3.26)

project(game)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_CXX_STANDARD 23)
# SETUP VARIABLES:

# list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/build/modules)
set(CMAKE_INCLUDE_PATH ${PROJECT_SOURCE_DIR}/inc)
set(CMAKE_INCLUDE_PATH ${PROJECT_SOURCE_DIR}/res)
set(OUTPUT_DIR "${CMAKE_SOURCE_DIR}/bin/${CMAKE_BUILD_TYPE}")
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${OUTPUT_DIR}")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${OUTPUT_DIR}")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${OUTPUT_DIR}")


# SETUP SDL2

# find_package(SDL2 REQUIRED)   # <<< probably best practice.
                                # But lets do it manually for now.
                                #   The downside is that if the development environment
                                #   differ from others (which they tend to do).
                                #   Therefore each developer must change the SDL2 paths to their SDL2 folder.
                                #   ...and libs is probably not compatible with all compilers.

# NOTE: Folders here contain both SDL2 and SDL2_image files.
set(SDL3_LIB_DIR D:/mingw64/x86_64-w64-mingw32/lib/sdl_lib)
set(SDL3_INCLUDE_DIR D:/mingw64/x86_64-w64-mingw32/include)
set(SDL3_LIBRARY D:/mingw64/x86_64-w64-mingw32/bin)

# SETUP INCLUDE AND LINK DIRS:


include_directories(
    ${SDL3_INCLUDE_DIR}
    ${PROJECT_SOURCE_DIR}/include
)

link_directories(
    ${SDL3_LIB_DIR}
)


# INCLUDE FILES THAT SHOULD BE COMPILED:
include_directories(${PROJECT_SOURCE_DIR}/inc)
include_directories(${PROJECT_SOURCE_DIR}/src)

file(GLOB_RECURSE SRC "src/*.cpp" "src/*.h")

message(STATUS "Add source files:")
foreach(SRC_FILE IN LISTS SRC)
    message(STATUS "${SRC_FILE}")
endforeach()
message(STATUS "")

add_executable(${PROJECT_NAME} ${SRC})


# LINK PROGRAM

target_link_libraries(
    	${PROJECT_NAME} PRIVATE ${PROJECT_SOURCE_DIR}/inc 
    	mingw32
        SDL3
	SDL3_image
	SDL3_ttf
)


# COPY OVER STATIC FILES

message(STATUS "Copying all assets to destination folder...")
file(
    COPY assets
    DESTINATION ${OUTPUT_DIR}
)

message(STATUS "Copying all dependencies to destination folder...")
file(GLOB PROJECT_DEPENDENCIES ${SDL3_LIBRARY}/*.dll)

foreach(Dependency IN LISTS PROJECT_DEPENDENCIES)
    file(
        COPY ${Dependency}
        DESTINATION ${OUTPUT_DIR}
    )
endforeach()
