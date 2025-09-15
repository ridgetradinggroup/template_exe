cmake_minimum_required(VERSION 3.16)
project({{ project_name }} VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD {{ cpp_standard }})
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Main executable
add_executable({{ project_name_underscore }}
    src/main.cpp
    # Add more source files here as your project grows
    # src/utils.cpp
    # src/config.cpp
)

# Set target properties
set_target_properties({{ project_name_underscore }} PROPERTIES
    OUTPUT_NAME {{ project_name_underscore }}
    DEBUG_POSTFIX d
)

# Include directories for any headers (if you add them later)
# target_include_directories({{ project_name_underscore }} PRIVATE
#     $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
# )

# Export compile features
target_compile_features({{ project_name_underscore }} PRIVATE cxx_std_{{ cpp_standard }})

# Installation setup
include(GNUInstallDirs)

# Install the executable
install(TARGETS {{ project_name_underscore }}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

# vcpkg dependency example (uncomment and modify as needed)
# find_package(fmt CONFIG REQUIRED)
# target_link_libraries({{ project_name_underscore }} PRIVATE fmt::fmt)

#-------------------------------------------------------------------------------
#
# Testing Support
#
#-------------------------------------------------------------------------------
{% if testing_enabled -%}
include(CTest)
option(BUILD_TESTING "Build the testing tree" ON)

if(BUILD_TESTING)
    enable_testing()
    
    # Basic executable test - check that it runs without errors
    add_test(
        NAME basic_run 
        COMMAND {{ project_name_underscore }}
    )
    
    # Test that executable produces expected output
    add_test(
        NAME output_test
        COMMAND {{ project_name_underscore }}
    )
    set_tests_properties(output_test PROPERTIES 
        PASS_REGULAR_EXPRESSION "Hello from {{ project_name }}!"
    )
    
    # Test executable exit code
    add_test(
        NAME exit_code_test
        COMMAND {{ project_name_underscore }}
    )
    set_tests_properties(exit_code_test PROPERTIES
        WILL_FAIL FALSE
    )
endif()
{% endif -%}