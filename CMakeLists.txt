cmake_minimum_required(VERSION 2.8)

project(infra_project)

# find BLAS and LAPACK
find_package ( BLAS )
find_package ( LAPACK )

SET(START_DIR .)

set( CMAKE_RUNTIME_OUTPUT_DIRECTORY ${START_DIR}/builds/win32 )
set( CMAKE_LIBRARY_OUTPUT_DIRECTORY ${START_DIR}/builds/win32/lib )
set( CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${START_DIR}/builds/win32/lib )

SET(INFRA_DIR ${START_DIR}/src)

add_library(infra STATIC ${INFRA_DIR}/infra_vv_funcs.cpp ${INFRA_DIR}/infra_vm_funcs.cpp ${INFRA_DIR}/infra_mm_funcs.cpp ${INFRA_DIR}/infra_exception.cpp ${INFRA_DIR}/infra_binary.cpp)
target_include_directories (infra PUBLIC ${INFRA_DIR})

# Compile tester executables

set(TESTERS tester tester_endian)

FOREACH(tester ${TESTERS})
	add_executable(${tester} ${START_DIR}/tester/${tester}.cpp)
	target_link_libraries(${tester} LINK_PUBLIC infra)
	IF(LAPACK_FOUND AND BLAS_FOUND)
		MESSAGE ( "-- " ${tester} ": Linking BLAS and LAPACK")
		target_link_libraries(${tester} LINK_PUBLIC BLAS)
		target_link_libraries(${tester} LINK_PUBLIC LAPACK)
	ENDIF(LAPACK_FOUND AND BLAS_FOUND)
ENDFOREACH()

install(TARGETS tester DESTINATION bin)
install(TARGETS tester_endian DESTINATION bin)