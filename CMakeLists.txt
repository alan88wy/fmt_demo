# add_executable(hello main.cpp)

# ----- 1. Runtime requirement 

cmake_minimum_required(VERSION 3.15)

set(CMAKE_C_COMPILER "gcc")
set(CMAKE_CXX_COMPILER "g++")

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Debug CACHE STRING "" FORCE)
endif()

set(CMAKE_CXX_STANDARD 23)
set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DVK_PROTOTYPES")
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DVK_PROTOTYPES")

# ----- 2. Debugging

# ----- 3. Global variable

set(this Hello)

project(${this} CXX C)

# Install fmt
include(FetchContent)

FetchContent_Declare(
  fmt
  GIT_REPOSITORY https://github.com/fmtlib/fmt
  GIT_TAG        e69e5f977d458f2650bb346dadf2ad30c5320281) # 10.2.1
FetchContent_MakeAvailable(fmt)

# Do this if using installed version
# find_package(fmt)
# target_link_libraries(<your-target> fmt::fmt)

# ----- 4. Software tool

# ----- 5. Library

# set(includes ${CMAKE_CURRENT_SOURCE_DIR}/imgui )
# include_directories( ${includes} )

set( sources ${CMAKE_CURRENT_SOURCE_DIR}/main.cpp)

# set(VULKAN_LIB_LIST "vulkan-1")

# set(VENDORS "./imgui")

# file(GLOB PROJECT_HEADERS imgui/*.h)
# file(GLOB PROJECT_SOURCES *.cpp imgui/*.cpp)
file(GLOB PROJECT_SOURCES *.cpp)

# source_group("Headers" FILES ${PROJECT_HEADERS})
# source_group("Shaders" FILES ${PROJECT_SHADERS})
source_group("Sources" FILES ${PROJECT_SOURCES})
# source_group("Vendors" FILES ${VENDORS_SOURCES})

# ----- 6. Compile

# add_executable( ${this} ${sources} )
# add_executable(${this} ${PROJECT_SOURCES} ${PROJECT_HEADERS} )
add_executable(${this} ${PROJECT_SOURCES}  )

# Adding Libraries 
if(MSVC OR MSYS OR MINGW)

#   find_package(fmt REQUIRED)
#   find_package(range-v3 REQUIRED)
#   find_package(cxxopts REQUIRED)
#   find_package(glfw3 REQUIRED)
#   find_package(glm REQUIRED)
#   find_package(GLUT REQUIRED)
#   find_package(vulkan REQUIRED)

  find_package(Poco REQUIRED Foundation)
  
else()
	include_directories(
		    # Headers/
                    # Vendors/assimp/include/
                    # Vendors/bullet/src/
                    # Vendors/glad/include/
                    # Vendors/glfw/include/
                    # Vendors/glm/
                    # Vendors/stb/
                    # $(Vendors)/include/

	)
	list(APPEND CMAKE_PREFIX_PATH "/usr/lib")

    find_package(Poco REQUIRED Foundation)

	# find_package(glfw3 REQUIRED)
	# find_package(Vulkan REQUIRED)
	# find_package(glm REQUIRED)

  # GLUT
#   find_package(GLUT REQUIRED)
#   include_directories(${GLUT_INCLUDE_DIR})
#   if(NOT GLUT_FOUND)
#       message(ERROR "GLUT not found!")
#   endif(NOT GLUT_FOUND)
endif()

if(${CMAKE_SYSTEM_NAME} MATCHES "Windows")
  target_link_libraries( ${this} PUBLIC ${target_lib} ) 
  target_include_directories( ${this} PUBLIC ${includes} ) 
#   target_link_libraries(${this}
#     PRIVATE
#       fmt::fmt
#       range-v3::range-v3
#       cxxopts::cxxopts
#       glfw
#       glm::glm
#       GLUT::GLUT
#       Vulkan::Vulkan
#  )
else()
    # target_link_libraries(${PROJECT_NAME} glfw)
    # target_link_libraries(${PROJECT_NAME} Vulkan::Vulkan)
    # target_link_libraries(${PROJECT_NAME} glm::glm)

  
endif()

target_link_libraries(${this} fmt::fmt)

# Adding dependencies eg Shaders etc
# add_dependencies(${PROJECT_NAME} assets)

# Define project properties
set_property(TARGET ${PROJECT_NAME} PROPERTY RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/bin)
set_property(TARGET ${PROJECT_NAME} PROPERTY RUNTIME_OUTPUT_DIRECTORY_DEBUG ${CMAKE_CURRENT_SOURCE_DIR}/bin)
set_property(TARGET ${PROJECT_NAME} PROPERTY RUNTIME_OUTPUT_DIRECTORY_RELEASE ${CMAKE_CURRENT_SOURCE_DIR}/bin)
set_property(TARGET ${PROJECT_NAME} PROPERTY RUNTIME_OUTPUT_DIRECTORY_MINSIZEREL ${CMAKE_CURRENT_SOURCE_DIR}/bin)
set_property(TARGET ${PROJECT_NAME} PROPERTY RUNTIME_OUTPUT_DIRECTORY_RELWITHDEBINFO ${CMAKE_CURRENT_SOURCE_DIR}/bin)

# Define C++ version to be used for building the project
set_property(TARGET ${PROJECT_NAME} PROPERTY CXX_STANDARD 20)
set_property(TARGET ${PROJECT_NAME} PROPERTY CXX_STANDARD_REQUIRED ON)

# Define C version to be used for building the project
set_property(TARGET ${PROJECT_NAME} PROPERTY C_STANDARD 99)
set_property(TARGET ${PROJECT_NAME} PROPERTY C_STANDARD_REQUIRED ON)