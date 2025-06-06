get_filename_component(_TEST_NAME ${CMAKE_CURRENT_LIST_DIR} NAME)
add_executable(${_TEST_NAME}_tests
    ${_TEST_NAME}_tests_main.cpp
    slic3r_jobs_tests.cpp
    slic3r_version_tests.cpp
    slic3r_arrangejob_tests.cpp
    secretstore_tests.cpp
    )

# mold linker for successful linking needs also to link TBB library and link it before libslic3r.
target_link_libraries(${_TEST_NAME}_tests test_common TBB::tbb TBB::tbbmalloc libslic3r_gui libslic3r libseqarrange)

if (MSVC)
    target_link_libraries(${_TEST_NAME}_tests Setupapi.lib)
endif ()
set_property(TARGET ${_TEST_NAME}_tests PROPERTY FOLDER "tests")

if (WIN32)
    prusaslicer_copy_dlls(${_TEST_NAME}_tests)
endif()

# catch_discover_tests(${_TEST_NAME}_tests TEST_PREFIX "${_TEST_NAME}: ")
set(_catch_args "exclude:[NotWorking];-s")
list(APPEND _catch_args "${CATCH_EXTRA_ARGS}")
add_test(${_TEST_NAME}_tests ${_TEST_NAME}_tests ${_catch_args})
