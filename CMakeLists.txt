project(MESS)
cmake_minimum_required(VERSION 3.0)
find_package(OpenMP REQUIRED)
find_package(PythonInterp REQUIRED)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
add_library(mess
    ${PROJECT_SOURCE_DIR}/src/libmess/atom.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/io.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/math.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/mpack.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/symmetry.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/d3.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/key.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/mess.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/multindex.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/units.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/graph_common.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/lapack.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/permutation.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/graph_omp.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/linpack.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/model.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/slatec.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/crossrate.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/random.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/read.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/divsur.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/dynamic.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/structure.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/configuration.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/monom.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/logical.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/potential.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/system.cc
    ${PROJECT_SOURCE_DIR}/src/libmess/trajectory.cc)
add_executable(messdr ${PROJECT_SOURCE_DIR}/src/mess_driver.cc)
add_executable(messpf ${PROJECT_SOURCE_DIR}/src/partition_function.cc)
add_executable(messabs ${PROJECT_SOURCE_DIR}/src/abstraction.cc)
add_library(messtools SHARED ${PROJECT_SOURCE_DIR}/src/messtools.cc)
set_target_properties(messdr PROPERTIES OUTPUT_NAME "mess")
set_target_properties(messtools PROPERTIES PREFIX "")
target_link_libraries(messdr
    mess blas lapack mlapack_qd mlapack_dd mblas_qd mblas_dd qd slatec dl)
target_link_libraries(messpf
    mess blas lapack mlapack_qd mlapack_dd mblas_qd mblas_dd qd slatec dl)
target_link_libraries(messabs
    mess blas lapack mlapack_qd mlapack_dd mblas_qd mblas_dd qd slatec dl)
target_link_libraries(messtools
    mess blas lapack mlapack_qd mlapack_dd mblas_qd mblas_dd qd slatec dl)
install(TARGETS messdr DESTINATION bin)
install(TARGETS messpf DESTINATION bin)
install(TARGETS messabs DESTINATION bin)
install(TARGETS messtools LIBRARY DESTINATION lib)