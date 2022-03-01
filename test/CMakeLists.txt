link_directories("/usr/local/lib")
add_executable(${PROJECT_NAME}.test
  AllTests.cpp
  ${PROJECT_NAME}Test.cpp)
target_link_libraries(${PROJECT_NAME}.test
  ${PROJECT_NAME} gtest gmock pthread)
target_compile_definitions(${PROJECT_NAME}.test
  PRIVATE TEST_DIR="${CMAKE_CURRENT_LIST_DIR}/test")
install(TARGETS ${PROJECT_NAME}.test
        DESTINATION "${tool_dest}")

# Mac OS X does not look at these paths
target_include_directories(${PROJECT_NAME}.test PRIVATE "/usr/local/include/")
set(CMAKE_LIBRARY_PATH "/usr/local/lib/")
