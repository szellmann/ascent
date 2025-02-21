# Copyright (c) Lawrence Livermore National Security, LLC and other Ascent
# Project developers. See top-level LICENSE AND COPYRIGHT files for dates and
# other details. No copyright assignment is required to contribute to Ascent.

set(subset_thirdparty_libs dray)
set(subset_thirdparty_libs_mpi dray_mpi)

if(ENABLE_CUDA)
    list(APPEND subset_thirdparty_libs cuda)
    list(APPEND subset_thirdparty_libs_mpi cuda)
endif()

################################################
# subset
################################################
if(ENABLE_SERIAL)
    blt_add_executable(NAME subset
                       SOURCES subset.cpp
                       DEPENDS_ON ${subset_thirdparty_libs}
                       OUTPUT_DIR ${CMAKE_CURRENT_BINARY_DIR}
    )
endif()


################################################
# MPI SUPPORT
################################################

if(MPI_FOUND)
    ################################################
    # subset mpi
    ################################################
    blt_add_executable(NAME subset_mpi
                       SOURCES subset.cpp
                       DEPENDS_ON ${subset_thirdparty_libs_mpi}
                       OUTPUT_DIR ${CMAKE_CURRENT_BINARY_DIR}
                       )

    target_compile_definitions(subset_mpi PRIVATE "MPI_ENABLED")

endif()

configure_file(subset_config.yaml ${CMAKE_CURRENT_BINARY_DIR}/subset_config.yaml COPYONLY)
