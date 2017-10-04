project(dmatrix)

set(DMATRIX_INCLUDE_DIRS
        ${PROJECT_SOURCE_DIR}/include
        ${LUSOL_INCLUDE_DIRS}
        ${CXSPARSE_INCLUDE_DIRS}
        CACHE INTERNAL "${PROJECT_NAME}: Include Directories" FORCE)

include_directories(${DMATRIX_INCLUDE_DIRS})
message(STATUS "DMATRIX_INCLUDE_DIRS ----> " "${DMATRIX_INCLUDE_DIRS}")

set(SOURCES
        src/dmatrixv.cxx)

SET(GCC_COVERAGE_COMPILE_FLAGS "${HOMER_PSOPT_GCC_COMPILE_FLASGS} -DLAPACK -DUNIX -DSPARSE_MATRIX -DUSE_IPOPT -fomit-frame-pointer -pipe -DNDEBUG -pedantic-errors -Wparentheses -Wreturn-type -Wcast-qual -Wall -Wpointer-arith -Wwrite-strings -Wconversion -fPIC -DHAVE_MALLOC")

SET(CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} ${GCC_COVERAGE_COMPILE_FLAGS}" )

link_directories(${IPOPT_LIBRARY_DIRS} ${LUSOL_LIBRARY_DIRS} ${CXSPARSE_LIBRARY_DIRS})
add_library(${PROJECT_NAME} STATIC ${SOURCES})
target_link_libraries(${PROJECT_NAME}
        ${IPOPT_LIBRARIES}
        ${LUSOL_LIBRARIES}
        ${CXSPARSE_LIBRARIES}
        dl)

# examples
project(dmatrix_colon)
link_directories(dmatrix)
add_executable(${PROJECT_NAME} examples/colon.cxx)
target_link_libraries(${PROJECT_NAME} dmatrix)

project(dmatrix_expm)
link_directories(dmatrix)
add_executable(${PROJECT_NAME} examples/expm.cxx)
target_link_libraries(${PROJECT_NAME} dmatrix)

project(dmatrix_sparse)
link_directories(dmatrix)
add_executable(${PROJECT_NAME} examples/sparse.cxx)
target_link_libraries(${PROJECT_NAME} dmatrix)