cmake_minimum_required(VERSION 3.2)

project(example-plugin)

include(GNUInstallDirs)

find_package(f3d REQUIRED)

f3d_plugin_init()

f3d_plugin_declare_reader(
  NAME ExampleReader
  EXTENSIONS expl
  VTK_READER vtkF3DExampleReader
  DESCRIPTION "Example reader"
)

f3d_plugin_build(
  NAME example
  VERSION 1.0
  DESCRIPTION "Example plugin"
  VTK_MODULES CommonCore CommonExecutionModel
)

if(BUILD_TESTING)
  enable_testing()
  add_test(NAME TestPluginExample
          COMMAND "$<TARGET_FILE:f3d::f3d>"
                  "--dry-run"
                  "--no-render"
                  "--load-plugins=$<TARGET_FILE:f3d-plugin-example>"
                  "${CMAKE_CURRENT_SOURCE_DIR}/data.expl"
  )

  set_tests_properties(TestPluginExample PROPERTIES
    PASS_REGULAR_EXPRESSION "Number of points: 3"
  )
endif()
