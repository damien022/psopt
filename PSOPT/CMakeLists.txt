project(psopt)
message(STATUS "Setting up PSOPT")
message(STATUS "DMATRIX_INCLUDE_DIRS ----> " "${DMATRIX_INCLUDE_DIRS}")

set(PSOPT_INCLUDE_DIRS
        ${PROJECT_SOURCE_DIR}/include
        ${DMATRIX_INCLUDE_DIRS}
        ${ADOLC_INCLUDE_DIRS}
        ${IPOPT_INCLUDE_DIRS}
        CACHE INTERNAL "${PROJECT_NAME}: Include Directories" FORCE)

include_directories(${PSOPT_INCLUDE_DIRS})
message(STATUS "PSOPT_INCLUDE_DIRS ----> " "${PSOPT_INCLUDE_DIRS}")
message(STATUS "ADOLC_INCLUDE_DIRS ----> " "${ADOLC_INCLUDE_DIRS}")

set(PSOPT_LIBRARY_DIRS
        ${IPOPT_LIBRARY_DIRS}
        ${LUSOL_LIBRARY_DIRS}
        ${CXSPARSE_LIBRARY_DIRS}
        ${ADOLC_LIBRARY_DIRS}
        CACHE INTERNAL "${PROJECT_NAME}: Library Directories" FORCE)

set(PSOPT_EXTERNAL_LIBRARIES
        ${IPOPT_LIBRARIES}
        ${LUSOL_LIBRARIES}
        ${CXSPARSE_LIBRARIES}
        ${ADOLC_LIBRARIES}
        dmatrix
        dl
        )

set(PSOPT_LIBRARIES
        ${IPOPT_LIBRARIES}
        ${LUSOL_LIBRARIES}
        ${CXSPARSE_LIBRARIES}
        ${ADOLC_LIBRARIES}
        dmatrix
        dl
        ${PROJECT_NAME}
        CACHE INTERNAL "${PROJECT_NAME}: Libraries" FORCE
        )

set(SOURCES
        src/derivatives.cxx
        src/evaluate.cxx
        src/get_numbers.cxx
        src/get_variables.cxx
        src/integrate.cxx
        src/interpolation.cxx
        src/IPOPT_interface.cxx
        src/mesh.cxx
        src/NLP_bounds.cxx
        src/NLP_constraints.cxx
        src/NLP_guess.cxx
        src/NLP_guess.cxx
        src/NLP_interface.cxx
        src/NLP_objective.cxx
        src/parameter_estimation.cxx
        src/phases.cxx
        src/plot.cxx
        src/print.cxx
        src/propagate.cxx
        src/pseudospectral.cxx
        src/psopt.cxx
        src/scaling.cxx
        src/setup.cxx
        src/solution.cxx
        src/trajectories.cxx
        src/user_functions.cxx
        src/util.cxx
        src/validate.cxx
        src/workspace.cxx
        include/ipopt_psopt.h
        include/ipopt_psoc.h
        include/psopt.h
        )

SET(PSOPT_GCC_COMPILE_FLASGS "${PSOPT_GCC_COMPILE_FLASGS} -DLAPACK -DUNIX -DSPARSE_MATRIX -DUSE_IPOPT -fomit-frame-pointer -pipe -DNDEBUG -Wparentheses -Wreturn-type -Wcast-qual -Wall -Wpointer-arith -Wwrite-strings -Wconversion -fPIC -DHAVE_MALLOC"
        CACHE INTERNAL "${PROJECT_NAME}: Compile Flags" FORCE)
SET( CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} ${PSOPT_GCC_COMPILE_FLASGS}" )

link_directories(${PSOPT_LIBRARY_DIRS})
add_library(${PROJECT_NAME} STATIC ${SOURCES})
target_link_libraries(${PROJECT_NAME} ${PSOPT_EXTERNAL_LIBRARIES})

add_subdirectory(examples)

# set the external libraries necessary for it all