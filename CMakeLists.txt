cmake_minimum_required(VERSION 3.3.1)
project(ccrypto C)

SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -msse4.1 -maes")

include_directories(${CMAKE_SOURCE_DIR}/src)

set (source_files
	src/aesb.c
	src/blake256.c
	#src/chacha.c
	src/crypto-ops-data.c
	src/crypto-ops.c
	src/groestl.c
	src/hash-extra-blake.c
	src/hash-extra-groestl.c
	src/hash-extra-jh.c
	src/hash-extra-skein.c
	src/hash.c
	src/jh.c
	src/keccak.c
	src/oaes_lib.c
	src/random.c
	src/skein.c
	src/slow-hash.c
	src/tree-hash.c
	)

if (ARM)
  option(NO_OPTIMIZED_MULTIPLY_ON_ARM
	   "Compute multiply using generic C implementation instead of ARM ASM" OFF)
  if(NO_OPTIMIZED_MULTIPLY_ON_ARM)
    message(STATUS "Using generic C implementation for multiply")
    set_property(SOURCE slow-hash.c
      PROPERTY COMPILE_DEFINITIONS "NO_OPTIMIZED_MULTIPLY_ON_ARM")
  endif()
endif()

add_library(libcncrypto SHARED
	${source_files}
	)

target_include_directories(libcncrypto PUBLIC ${CMAKE_SOURCE_DIR})
