cmake_minimum_required(VERSION 3.20)

set(PATCH_VERSION "1" CACHE INTERNAL "Patch version")
set(PROJECT_VERSION 0.0.${PATCH_VERSION})

project(fashio_mnist VERSION ${PROJECT_VERSION})

configure_file(version.h.in version.h)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

find_package(Boost COMPONENTS system REQUIRED)

if (MSVC)
	add_compile_options(/W4)
	set(TENSORFLOW_URL "https://storage.googleapis.com/tensorflow/libtensorflow/libtensorflow-cpu-windows-x86_64-2.10.0.zip")
else()
	add_compile_options(-Wall -Wextra -pedantic -Werror)
	set(TENSORFLOW_URL "https://storage.googleapis.com/tensorflow/libtensorflow/libtensorflow-cpu-linux-x86_64-2.14.0.tar.gz")
endif()

include(FetchContent)
FetchContent_Declare(
	tensorflow
	URL ${TENSORFLOW_URL}
)
FetchContent_MakeAvailable(tensorflow)

add_subdirectory(library)

install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION bin)

set(CPACK_GENERATOR DEB)
set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")
set(CPACK_PACKAGE_CONTACT example@example.com)
include(CPack)