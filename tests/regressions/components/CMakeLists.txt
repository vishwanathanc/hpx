# Copyright (c) 2007-2017 Hartmut Kaiser
#
# Distributed under the Boost Software License, Version 1.0. (See accompanying
# file LICENSE_1_0.txt or copy at http://www.boost.org/LICENSE_1_0.txt)

set(tests
    bulk_new_3054
    client_base_registration
    create_n_components_2323
    create_remote_component_2334
    moveonly_constructor_arguments_1405
    new_2848
    partitioned_vector_2201
    returned_client_2150
   )

set(bulk_new_3054_PARAMETERS LOCALITIES 2)
set(create_remote_component_2334_PARAMETERS LOCALITIES 2)
set(new_2848_PARAMETERS LOCALITIES 2)

foreach(test ${tests})
  set(sources
      ${test}.cpp)

  source_group("Source Files" FILES ${sources})

  # add example executable
  add_hpx_executable(${test}_test
                     SOURCES ${sources}
                     ${${test}_FLAGS}
                     EXCLUDE_FROM_ALL
                     HPX_PREFIX ${HPX_BUILD_PREFIX}
                     FOLDER "Tests/Regressions/Components")

  add_hpx_regression_test("components_dir" ${test} ${${test}_PARAMETERS})

  # add a custom target for this test
  add_hpx_pseudo_target(tests.regressions.components_dir.${test})

  # make pseudo-targets depend on master pseudo-target
  add_hpx_pseudo_dependencies(tests.regressions.components_dir
                              tests.regressions.components_dir.${test})

  # add dependencies to pseudo-target
  add_hpx_pseudo_dependencies(tests.regressions.components_dir.${test}
                              ${test}_test_exe)
endforeach()

