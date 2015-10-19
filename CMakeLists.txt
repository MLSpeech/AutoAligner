cmake_minimum_required(VERSION 2.8)

project(forced_alignment_project)

SET(START_DIR .)

set( CMAKE_RUNTIME_OUTPUT_DIRECTORY ${START_DIR}/builds/win32 )
set( CMAKE_LIBRARY_OUTPUT_DIRECTORY ${START_DIR}/builds/win32/lib )
set( CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${START_DIR}/builds/win32/lib )

# Importing Infra

# find BLAS and LAPACK
find_package ( BLAS )
find_package ( LAPACK )

SET(INFRA_DIR ${START_DIR}/libs/infra/src)
add_library(infra STATIC ${INFRA_DIR}/infra_vv_funcs.cpp ${INFRA_DIR}/infra_vm_funcs.cpp ${INFRA_DIR}/infra_mm_funcs.cpp ${INFRA_DIR}/infra_exception.cpp ${INFRA_DIR}/infra_binary.cpp)
target_include_directories (infra PUBLIC ${INFRA_DIR})
	IF(LAPACK_FOUND AND BLAS_FOUND)
		MESSAGE ( "-- infra: Linking BLAS and LAPACK")
		target_link_libraries(infra LINK_PUBLIC BLAS)
		target_link_libraries(infra LINK_PUBLIC LAPACK)
	ENDIF(LAPACK_FOUND AND BLAS_FOUND)
	
# Importing cmd_line
SET(CMD_LINE_DIR ${START_DIR}/libs/cmd_line/src)
add_library(cmd_line STATIC ${CMD_LINE_DIR}/cmd_line.cpp)
target_include_directories (cmd_line PUBLIC ${CMD_LINE_DIR})

# Importing active_set
# SET(ACTIVE_SET_DIR ${START_DIR}/src)
# add_library(active_set STATIC ${ACTIVE_SET_DIR}/active_set.h)

# Importing forced_alignment
SET(FORCED_ALIGNMENT_DIR ${START_DIR}/src)
add_library(forced_alignment STATIC ${FORCED_ALIGNMENT_DIR}/Classifier.cpp
							${FORCED_ALIGNMENT_DIR}/Dataset.cpp)

target_link_libraries(forced_alignment LINK_PUBLIC infra)
target_include_directories(forced_alignment PUBLIC ${START_DIR}/include)

# Executeable 1: Decoder

add_executable(ForcedAlignmentDecode ${START_DIR}/src/ForcedAlignmentDecode.cpp)

target_link_libraries(ForcedAlignmentDecode LINK_PUBLIC cmd_line)
target_link_libraries(ForcedAlignmentDecode LINK_PUBLIC infra)
target_link_libraries(ForcedAlignmentDecode LINK_PUBLIC forced_alignment)

# Executeable 2: Trainer

add_executable(ForcedAlignmentTrain ${START_DIR}/src/ForcedAlignmentTrain.cpp)

target_link_libraries(ForcedAlignmentTrain LINK_PUBLIC cmd_line)
target_link_libraries(ForcedAlignmentTrain LINK_PUBLIC infra)
target_link_libraries(ForcedAlignmentTrain LINK_PUBLIC forced_alignment)

install(TARGETS ForcedAlignmentDecode DESTINATION bin)