# ------------------------------------------------------------------------------

# ADSK (using as child)
# project(UnitTest++)
message("Processing 3p/unittest++...")

# ------------------------------------------------------------------------------

# set modules name
set(MODULE_NAME UnitTest++)

# ------------------------------------------------------------------------------

option(UTPP_USE_PLUS_SIGN "Set this to OFF if you wish to use '-cpp' instead of '++' in lib/include paths" ON)

# ------------------------------------------------------------------------------

# ADSK, JNH, removed not required
# if(MSVC14 OR MSVC12)
#     # has the support we need
# else()
#     include(CheckCXXCompilerFlag)
#     CHECK_CXX_COMPILER_FLAG("-std=c++14" COMPILER_SUPPORTS_CXX14)
#     CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
#     if(COMPILER_SUPPORTS_CXX14)
#         set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14")
#     elseif(COMPILER_SUPPORTS_CXX11)
#         set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
#     else()
#             message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
#     endif()
# endif()

# ------------------------------------------------------------------------------

# get the main sources
file(GLOB headers_ RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} UnitTest++/*.h)
file(GLOB sources_ RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} UnitTest++/*.cpp)
source_group("" FILES ${headers_} ${sources_})

# ------------------------------------------------------------------------------

# get platform specific sources
if (WIN32 OR WIN64)
    add_definitions(-D_CRT_SECURE_NO_DEPRECATE)
    set(platformDir_ Win32)
else()
    set(platformDir_ Posix)
endif()

# ------------------------------------------------------------------------------

file(GLOB platformHeaders_ RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} UnitTest++/${platformDir_}/*.h)
file(GLOB platformSources_ RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} UnitTest++/${platformDir_}/*.cpp)
source_group(${platformDir_} FILES ${platformHeaders_} ${platformSources_})

# ------------------------------------------------------------------------------

# create the lib
add_library(${MODULE_NAME} ${PREFERRED_LIBRARY_TYPE} ${headers_} ${sources_} ${platformHeaders_} ${platformSources_})

# ------------------------------------------------------------------------------

# ADSK, mark as building 
target_compile_definitions(${MODULE_NAME}
	PRIVATE
		-DUNITTEST_BUILDING
)

# ------------------------------------------------------------------------------

# ADSK, public includes
target_include_directories(${MODULE_NAME}
	PUBLIC
		${CMAKE_CURRENT_SOURCE_DIR}/UnitTest++
)

# ------------------------------------------------------------------------------

if(${UTPP_USE_PLUS_SIGN})
	set_target_properties(${MODULE_NAME} PROPERTIES OUTPUT_NAME UnitTest++)
endif()

# ------------------------------------------------------------------------------

# ADSK, JNH, not required
# build the test runner
# file(GLOB TEST_SRCS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} tests/*.cpp tests/*.h)
# source_group( "" FILES ${TEST_SRCS})
# add_executable(TestUnitTest++ ${TEST_SRCS})
# include_directories(.)
# 
# if(${UTPP_USE_PLUS_SIGN})
# 	set_target_properties(TestUnitTest++ PROPERTIES OUTPUT_NAME TestUnitTest++)
# endif()
# 
# target_link_libraries(TestUnitTest++ UnitTest++)
# 
# # run unit tests as post build step
# add_custom_command(TARGET TestUnitTest++
# 	POST_BUILD COMMAND TestUnitTest++
# 	COMMENT "Running unit tests")
# 
# # add install targets
# # need a custom install path?
# # define CMAKE_INSTALL_PREFIX to change root folder
# if(${UTPP_USE_PLUS_SIGN})
# 	set (UTPP_INSTALL_DESTINATION "include/UnitTest++")
# else()
# 	set (UTPP_INSTALL_DESTINATION "include/UnitTestPP")
# endif()
# 
# set(config_install_dir_ lib/cmake/${PROJECT_NAME})
# set(targets_export_name_ "${PROJECT_NAME}Targets")
# 
# install(TARGETS UnitTest++ EXPORT "${targets_export_name_}" DESTINATION lib)
# install(FILES ${headers_} DESTINATION ${UTPP_INSTALL_DESTINATION})
# install(FILES ${platformHeaders_} DESTINATION ${UTPP_INSTALL_DESTINATION}/${platformDir_})
# install(FILES cmake/UnitTest++Config.cmake DESTINATION "${config_install_dir_}")
# install(EXPORT "${targets_export_name_}" DESTINATION "${config_install_dir_}")

# ------------------------------------------------------------------------------

# ADSK 
if(COMMAND apply_target_settings)
	apply_target_settings(${MODULE_NAME})
endif()

# ------------------------------------------------------------------------------

# Tidy up solution 
set_property(GLOBAL PROPERTY USE_FOLDERS ON)
set_property(TARGET ${MODULE_NAME} PROPERTY FOLDER "3p" )

# ------------------------------------------------------------------------------

