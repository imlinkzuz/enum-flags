include(${SIDECMAKE_DIR}/SCTest.cmake)

sc_add_unit_test(
  TEST_NAME unit_test 
  TEST_FRAMEWORK 
    BOOST 
  TARGETS
    enum-flags::enum-flags 
  TEST_SOURCES
      main-test.cpp
)

file(GLOB_RECURSE _shouldnt_compile_test_sources CONFIGURE_DEPENDS "./shouldnt-compile/*.cpp")
foreach(_source_file IN LISTS _shouldnt_compile_test_sources)
  get_filename_component(_filename_we ${_source_file} NAME_WE)
  sc_add_compile_only_test(
    TEST_NAME test_shouldnt_compile_${_filename_we} 
    WILL_FAIL
    TARGETS
        enum-flags::enum-flags 
    TEST_SOURCES
       ${_source_file} 
  )
endforeach()


sc_add_compile_only_test(
  TEST_NAME test_should_compile 
  TARGETS
    enum-flags::enum-flags 
  TEST_SOURCES
    should-compile.cpp 
)