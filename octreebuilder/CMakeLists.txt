set(target octreebuilder)

##########################################################################################################
# Lib Setup
##########################################################################################################

set(OCTREEBUILDER_USE_SSE ${BUILD_WITH_SSE})
set(NO_EDIT "AUTO GENERATED. DO NOT EDIT!")

configure_file(build_options.h.in ${CMAKE_BINARY_DIR}/include/build_options.h)
install(FILES ${CMAKE_BINARY_DIR}/include/build_options.h DESTINATION ${INSTALL_INCLUDE_DIR}/octreebuilder)

set(SOURCES
    octree.cpp
    octreebuilder.cpp
    octree_impl.cpp
    octreenode.cpp
    box.cpp
    linearoctree.cpp
    mortoncode_utils.cpp
    octantid.cpp
    octree_utils.cpp
    paralleloctreebuilder.cpp
    sequentialoctreebuilder.cpp
    vector3i.cpp
    vector_utils.cpp
    perfcounter.cpp
)

set(PUBLIC_HEADER
    mortoncode.h
    octree.h
    octreebuilder.h
    octreenode.h
    box.h
    octreebuilder_api.h
    paralleloctreebuilder.h
    sequentialoctreebuilder.h
    vector3i.h
    vector_utils.h
)

set(HEADER
    ${PUBLIC_HEADER}

    build_options.h.in
    linearoctree.h
    mortoncode_utils.h
    octantid.h
    octree_impl.h
    octree_utils.h
    parallel_stable_sort.h
    perfcounter.h
)

add_library(${target} ${LIBRARY_TYPE} ${HEADER} ${SOURCES})


include_directories(
    ${PROJECT_BINARY_DIR}
)

include_directories(
    BEFORE
    ${CMAKE_CURRENT_SOURCE_DIR}
)

set_target_properties(${target}
    PROPERTIES
    LINKER_LANGUAGE             CXX
    FOLDER                      ""
    COMPILE_DEFINITIONS                 "${DEFAULT_COMPILE_DEFS}"
    COMPILE_DEFINITIONS_DEBUG           "${DEFAULT_COMPILE_DEFS_DEBUG}"
    COMPILE_DEFINITIONS_RELEASE         "${DEFAULT_COMPILE_DEFS_RELEASE}"
    COMPILE_DEFINITIONS_RELWITHDEBINFO  "${DEFAULT_COMPILE_DEFS_RELWITHDEBINFO}"
    LINK_FLAGS                          "${DEFAULT_LINKER_FLAGS}"
    LINK_FLAGS_DEBUG                    "${DEFAULT_LINKER_FLAGS_DEBUG}"
    LINK_FLAGS_RELEASE                  "${DEFAULT_LINKER_FLAGS_RELEASE}"
    LINK_FLAGS_RELWITHDEBINFO           "${DEFAULT_LINKER_FLAGS_RELWITHDEBINFO}"
    STATIC_LIBRARY_FLAGS_DEBUG          "${DEFAULT_LIBRARIAN_FLAGS_DEBUG}"
    STATIC_LIBRARY_FLAGS_RELEASE        "${DEFAULT_LIBRARIAN_FLAGS_RELEASE}"
    STATIC_LIBRARY_FLAGS_RELWITHDEBINFO "${DEFAULT_LIBRARIAN_FLAGS_RELWITHDEBINFO}"
    DEBUG_POSTFIX               "d")


target_compile_options(${target} PRIVATE "${DEFAULT_COMPILE_FLAGS}")

target_link_libraries(${target} ${TCMALLOC_LIBRARIES} ${GPERFTOOLS_LIBRARIES})

IF (BUILD_TESTS)
    add_subdirectory(unittests)
    add_subdirectory(integrationtests)
ENDIF()

add_definitions(-DOCTREEBUILDER_EXPORTS)
install_module(${target})
