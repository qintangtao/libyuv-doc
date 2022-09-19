cmake_minimum_required(VERSION 2.8)

SET (CMAKE_C_COMPILER             	"clang")
SET (CMAKE_C_FLAGS                	"-Wall -std=c99")
SET (CMAKE_C_FLAGS_DEBUG          	"-g")
SET (CMAKE_C_FLAGS_MINSIZEREL     	"-Os -DNDEBUG")
SET (CMAKE_C_FLAGS_RELEASE        	"-O4 -DNDEBUG")
SET (CMAKE_C_FLAGS_RELWITHDEBINFO 	"-O2 -g")
 
SET (CMAKE_CXX_COMPILER             "clang++")
SET (CMAKE_CXX_FLAGS                "-Wall -std=c++11")
SET (CMAKE_CXX_FLAGS_DEBUG          "-g")
SET (CMAKE_CXX_FLAGS_MINSIZEREL     "-Os -DNDEBUG")
SET (CMAKE_CXX_FLAGS_RELEASE        "-O4 -DNDEBUG")
SET (CMAKE_CXX_FLAGS_RELWITHDEBINFO "-O2 -g")
 
SET (CMAKE_AR      "llvm-ar")
SET (CMAKE_LINKER  "llvm-ld")
SET (CMAKE_NM      "llvm-nm")
SET (CMAKE_OBJDUMP "llvm-objdump")
SET (CMAKE_RANLIB  "llvm-ranlib")


#project(yuv)


#find_package(LLVM REQUIRED CONFIG)

SET(CMAKE_BUILD_TYPE Release)

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#set(CMAKE_CXX_STANDARD 11)
set(CMAKE_VERBOSE_MAKEFILE ON)


SET ( ly_base_dir	${PROJECT_SOURCE_DIR} )
SET ( ly_src_dir	${ly_base_dir}/src )
SET ( ly_inc_dir	${ly_base_dir}/include )
SET ( ly_lib_name	yuv )
SET ( ly_lib_static	${ly_lib_name} )
SET ( ly_lib_shared	${ly_lib_name}_shared )

FILE ( GLOB_RECURSE	ly_source_files ${ly_src_dir}/*.cc )
LIST ( SORT			ly_source_files )

INCLUDE_DIRECTORIES( 
BEFORE ${ly_inc_dir} 
${LLVM_INCLUDE_DIRS}
)

#include_directories(
#${PROJECT_SOURCE_DIR}/include
#${LLVM_INCLUDE_DIRS}
#)


#add_definitions(${LLVM_DEFINITIONS})

#add_library(yuv SHARED ${ly_source_files} ${PROJECT_SOURCE_DIR}/lib/libyuv.def)
#add_library(yuv SHARED ${ly_source_files})
#target_link_libraries(yuv -fPIC -Wl,-Bsymbolic ${llvm_libs})

# this creates the static library (.a)
ADD_LIBRARY				( ${ly_lib_static} STATIC ${ly_source_files} )

# this creates the shared library (.so)
ADD_LIBRARY				( ${ly_lib_shared} SHARED ${ly_source_files} )
target_link_libraries(${ly_lib_shared} -fPIC -Wl,-Bsymbolic ${llvm_libs} gcc)
SET_TARGET_PROPERTIES	( ${ly_lib_shared} PROPERTIES OUTPUT_NAME "${ly_lib_name}" )
SET_TARGET_PROPERTIES	( ${ly_lib_shared} PROPERTIES PREFIX "lib" )