

file(GLOB_RECURSE BOOST_SOURCE_CODE ${CMAKE_CURRENT_SOURCE_DIR}/BOOST/*.cpp ${CMAKE_CURRENT_SOURCE_DIR}/BOOST*.c)


macro(add_fcl_test test_name)
  add_executable(${ARGV})
  target_link_libraries(${test_name}
    fcl
	)
  add_test(${test_name} ${EXECUTABLE_OUTPUT_PATH}/${test_name})
endmacro(add_fcl_test)

# configure location of resources
file(TO_NATIVE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/fcl_resources" TEST_RESOURCES_DIR)
if(WIN32)
    # Correct directory separator for Windows
    string(REPLACE "\\" "\\\\" TEST_RESOURCES_DIR ${TEST_RESOURCES_DIR})
endif(WIN32)
configure_file("${TEST_RESOURCES_DIR}/config.h.in" "${TEST_RESOURCES_DIR}/config.h")

include_directories(.)

add_fcl_test(test_fcl_collision test_fcl_collision.cpp test_fcl_utility.cpp ${BOOST_SOURCE_CODE} )
add_fcl_test(test_fcl_distance test_fcl_distance.cpp test_fcl_utility.cpp ${BOOST_SOURCE_CODE})
add_fcl_test(test_fcl_geometric_shapes test_fcl_geometric_shapes.cpp test_fcl_utility.cpp ${BOOST_SOURCE_CODE})
add_fcl_test(test_fcl_broadphase test_fcl_broadphase.cpp test_fcl_utility.cpp ${BOOST_SOURCE_CODE})
add_fcl_test(test_fcl_shape_mesh_consistency test_fcl_shape_mesh_consistency.cpp test_fcl_utility.cpp ${BOOST_SOURCE_CODE})
add_fcl_test(test_fcl_frontlist test_fcl_frontlist.cpp test_fcl_utility.cpp ${BOOST_SOURCE_CODE})
add_fcl_test(test_fcl_math test_fcl_math.cpp test_fcl_utility.cpp ${BOOST_SOURCE_CODE})

if (FCL_HAVE_OCTOMAP)
  add_fcl_test(test_fcl_octomap test_fcl_octomap.cpp test_fcl_utility.cpp ${BOOST_SOURCE_CODE})
endif()
