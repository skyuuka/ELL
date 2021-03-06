#
# CMake file for Raspberry Pi tests
#

if(${PYTHON_ENABLED})

    set(module_name "pitest")
    set(test_name ${module_name}_test)

    set(test_src pitest.py )
    set(src ${test_src} drivetest.py runtest.sh.in coffeemug.jpg buildtask.cmd)

    add_custom_target(${module_name} DEPENDS SOURCES ${src})
    add_dependencies(${module_name} pythonlibs _ELL_python)
    
    set_property(TARGET ${module_name} PROPERTY FOLDER "tests")
    
    # copy files
    foreach(FILE ${src})
        configure_file(${FILE} ${FILE} COPYONLY)
    endforeach()

    if(RPI_CLUSTER AND RPI_PASSWORD AND RPI_KEY AND GIT_REPO)
        add_test(NAME ${test_name} COMMAND ${PYTHON_EXECUTABLE} ${test_src} 
                                            --cluster ${RPI_CLUSTER} 
                                            --password ${RPI_PASSWORD}
                                            --key ${RPI_KEY}
                                            --gitrepo ${GIT_REPO})
    endif()

endif()  # PYTHON_ENABLED
