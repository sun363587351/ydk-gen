cmake_minimum_required(VERSION 2.8.9)
cmake_policy(SET CMP0048 NEW)
project(ydk VERSION 0.5.2 LANGUAGES C CXX)


set(YDK_DESCRIPTION "YANG Development Kit Library. The library for  YDK API.")

# set version
set(YDK_MAJOR_VERSION 0)
set(YDK_MINOR_VERSION 5)
set(YDK_SERVICE_VERSION 2)
set(YDK_VERSION ${YDK_MAJOR_VERSION}.${YDK_MINOR_VERSION}.${YDK_SERVICE_VERSION})
set(YDK_CORE_SOVERSION ${YDK_MAJOR_VERSION}.${YDK_MINOR_VERSION})

# include custom Modules
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/CMakeModules/")

if(NOT LIB_INSTALL_DIR)
    set(LIB_INSTALL_DIR lib)
endif()

if(NOT INCLUDE_INSTALL_DIR)
    set(INCLUDE_INSTALL_DIR include/ydk)
endif()

# set default build type if not specified by user
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE debug)
endif()

set(CMAKE_CXX_FLAGS         "${CMAKE_CXX_FLAGS} -Wall -Wextra")
set(CMAKE_CXX_FLAGS_RELEASE "-O2 -DNDEBUG")
set(CMAKE_CXX_FLAGS_DEBUG   "-g -O0")

if(CMAKE_BUILD_TYPE STREQUAL debug)
    option(ENABLE_BUILD_TESTS "Build tests" ON)
    option(ENABLE_VALGRIND_TESTS "Build tests with valgrind" ON)
else()
    option(ENABLE_BUILD_TESTS "Build tests" OFF)
    option(ENABLE_VALGRIND_TESTS "Build tests with valgrind" OFF)
endif()

# Set LibYDK variables
set(libydk_src
    src/bits.cpp
    src/executor_service.cpp
    src/netconf_service.cpp
    src/ietf_netconf.cpp
    src/codec_provider.cpp
    src/codec_service.cpp
    src/crud_service.cpp
    src/entity.cpp
    src/entity_data_node_walker.cpp
    src/entity_lookup.cpp
    src/entity_util.cpp
    src/errors.cpp
    src/leaf_data.cpp
    src/netconf_client.cpp
    src/netconf_edit_operations.cpp
    src/netconf_model_provider.cpp
    src/netconf_provider.cpp
    src/validation_service.cpp
    src/value.cpp
    src/value_list.cpp
    src/ydk_yang.cpp
    src/path/annotation.cpp
    src/path/capability.cpp
    src/path/path.cpp
    src/path/data_node.cpp
    src/path/repository.cpp
    src/path/root_data_node.cpp
    src/path/root_schema_node.cpp
    src/path/rpc.cpp
    src/path/schema_node.cpp
    src/path/schema_value_type.cpp
    src/path/statement.cpp)

set(libydk_install_headers
    src/service.hpp
    src/executor_service.hpp
    src/netconf_service.hpp
    src/ietf_netconf.hpp
    src/ietf_netconf_with_defaults.hpp
    src/netconf_provider.hpp
    src/codec_service.hpp
    src/codec_provider.hpp
    src/entity_lookup.hpp
    src/entity_util.hpp
    src/path_api.hpp
    src/crud_service.hpp
    src/errors.hpp
    src/types.hpp
    src/service.hpp
    src/validation_service.hpp)

# Set LibNetconf variables
set(libnetconf_location ${CMAKE_CURRENT_BINARY_DIR}/project_libnetconf/src/project_libnetconf)

set(libnetconf_headers
    ${libnetconf_location}/src/callbacks.h
    ${libnetconf_location}/src/callbacks_ssh.h
    ${libnetconf_location}/src/callhome.h
    ${libnetconf_location}/src/config.h
    ${libnetconf_location}/src/config.h.in
    ${libnetconf_location}/src/datastore.h
    ${libnetconf_location}/src/datastore_xml.h
    ${libnetconf_location}/src/error.h
    ${libnetconf_location}/src/libnetconf.h
    ${libnetconf_location}/src/libnetconf_ssh.h
    ${libnetconf_location}/src/libnetconf_tls.h
    ${libnetconf_location}/src/libnetconf_xml.h
    ${libnetconf_location}/src/messages.h
    ${libnetconf_location}/src/messages_internal.h
    ${libnetconf_location}/src/messages_xml.h
    ${libnetconf_location}/src/nacm.h
    ${libnetconf_location}/src/netconf.h
    ${libnetconf_location}/src/netconf_internal.h
    ${libnetconf_location}/src/notifications.h
    ${libnetconf_location}/src/notifications_xml.h
    ${libnetconf_location}/src/session.h
    ${libnetconf_location}/src/ssh.h
    ${libnetconf_location}/src/tls.h
    ${libnetconf_location}/src/transapi.h
    ${libnetconf_location}/src/transport.h
    ${libnetconf_location}/src/url.h
    ${libnetconf_location}/src/url_internal.h
    ${libnetconf_location}/src/with_defaults.h
    ${libnetconf_location}/src/datastore/datastore_internal.h
    ${libnetconf_location}/src/datastore/edit_config.h
    ${libnetconf_location}/src/datastore/custom/datastore_custom.h
    ${libnetconf_location}/src/datastore/custom/datastore_custom_private.h
    ${libnetconf_location}/src/datastore/empty/datastore_empty.h
    ${libnetconf_location}/src/datastore/file/datastore_file.h
)

set(netconf_objs
    callbacks.o
    compat.o
    datastore.o
    datastore_custom.o
    datastore_empty.o
    datastore_file.o
    edit_config.o
    error.o
    internal.o
    messages.o
    nacm.o
    notifications.o
    session.o
    ssh.o
    transapi.o
    transport.o
    url.o
    with_defaults.o
    xmldiff.o
    yinparser.o
)

# Set LibYang variables
set(libyang_location ${CMAKE_CURRENT_BINARY_DIR}/project_libyang/src/project_libyang)

set(libyang_headers
    ${libyang_location}/src/common.h
    ${libyang_location}/src/context.h
    ${libyang_location}/src/dict.h
    ${libyang_location}/src/dict_private.h
    ${libyang_location}/src/libyang.h
    ${libyang_location}/src/parser.h
    ${libyang_location}/src/parser_yang.h
    ${libyang_location}/src/parser_yang_bis.h
    ${libyang_location}/src/parser_yang_lex.h
    ${libyang_location}/src/printer.h
    ${libyang_location}/src/resolve.h
    ${libyang_location}/src/tree_data.h
    ${libyang_location}/src/tree_internal.h
    ${libyang_location}/src/tree_schema.h
    ${libyang_location}/src/validation.h
    ${libyang_location}/src/xml.h
    ${libyang_location}/src/xml_internal.h
    ${libyang_location}/src/xpath.h
)

set(yang_objs
    common.c.o
    context.c.o
    dict.c.o
    log.c.o
    parser.c.o
    parser_json.c.o
    parser_xml.c.o
    parser_yang.c.o
    parser_yang_bis.c.o
    parser_yang_lex.c.o
    parser_yin.c.o
    printer.c.o
    printer_info.c.o
    printer_json.c.o
    printer_tree.c.o
    printer_xml.c.o
    printer_yang.c.o
    printer_yin.c.o
    resolve.c.o
    tree_data.c.o
    tree_schema.c.o
    validation.c.o
    xml.c.o
    xpath.c.o
    yang_types.c.o
)

set(Boost_USE_STATIC_LIBS        OFF)
set(Boost_USE_MULTITHREADED      ON)
set(Boost_USE_STATIC_RUNTIME     OFF)
set(BOOST_ALL_DYN_LINK           ON)   # force dynamic linking for all libraries
set(Boost_NO_BOOST_CMAKE         ON)
#set(Boost_USE_STATIC_LIBS ON)

# Find dependent libraries
find_library(xml2_location xml2)
find_library(curl_location curl)
find_library(ssh_threads_location ssh_threads)
find_library(xslt_location xslt)
find_library(pcre_location pcre)
find_library(pthread_location pthread)
find_library(dl_location dl)
find_package(Boost REQUIRED)
find_package(Boost COMPONENTS log_setup log thread date_time system filesystem REQUIRED)
find_package(LibSSH REQUIRED)

# Add LibNetconf
set(libnetconf_include_location include/libnetconf)
set(libnetconf_headers_location ${CMAKE_CURRENT_BINARY_DIR}/project_libnetconf/headers)
include(ExternalProject)
ExternalProject_Add(project_libnetconf
    PREFIX "${CMAKE_CURRENT_BINARY_DIR}/project_libnetconf"
    URL "https://github.com/abhikeshav/libnetconf/archive/master.zip"
    CONFIGURE_COMMAND "./configure"
    BUILD_COMMAND "make"
    INSTALL_DIR "${libnetconf_location}"
    INSTALL_COMMAND mkdir -p ${libnetconf_headers_location}/libnetconf && cp ${libnetconf_headers} ${libnetconf_headers_location}/libnetconf
    BUILD_IN_SOURCE 1
)
install(FILES ${libnetconf_headers} DESTINATION ${libnetconf_include_location})

ExternalProject_Get_Property(project_libnetconf install_dir)
set(libnetconf_archive ${install_dir}/.libs/libnetconf.a)

add_custom_command(
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/project_libnetconf/objs
    COMMAND cmake -E make_directory ${CMAKE_CURRENT_BINARY_DIR}/project_libnetconf/objs
)
set (libnetconf_objs ${CMAKE_CURRENT_BINARY_DIR}/project_libnetconf/objs)

list(APPEND libnetconf_inc_objs "")
foreach(netconf_obj IN LISTS netconf_objs)
    add_custom_command(
        OUTPUT ${libnetconf_objs}/${netconf_obj}
        COMMAND ar -x ${libnetconf_archive} ${netconf_obj}
        MAIN_DEPENDENCY ${libnetconf_objs}
        WORKING_DIRECTORY ${libnetconf_objs}
    )
    list(APPEND libnetconf_inc_objs ${libnetconf_objs}/${netconf_obj})
endforeach(netconf_obj)

add_library(libnetconf STATIC IMPORTED)
set_property(TARGET libnetconf PROPERTY IMPORTED_LOCATION ${libnetconf_archive})
add_dependencies(libnetconf project_libnetconf)

# Add LibYang
set(libyang_include_location include/libyang)
set(libyang_headers_location ${CMAKE_CURRENT_BINARY_DIR}/project_libyang/headers)
ExternalProject_Add(project_libyang
    PREFIX "${CMAKE_CURRENT_BINARY_DIR}/project_libyang"
    URL "https://github.com/manradhaCisco/libyang/archive/master.zip"
    CMAKE_ARGS -DSTATIC=true -DCMAKE_BUILD_TYPE:STRING=release
    INSTALL_DIR "${libyang_location}"
    INSTALL_COMMAND mkdir -p ${libyang_headers_location}/libyang && cp ${libyang_headers} ${libyang_headers_location}/libyang
)
install(FILES ${libyang_headers} DESTINATION ${libyang_include_location})

ExternalProject_Get_Property(project_libyang install_dir)
set(libyang_archive ${CMAKE_CURRENT_BINARY_DIR}/project_libyang/src/project_libyang-build/libyang.a)
# message("INSTALLING YANG TO: " ${install_dir})

add_library(libyang STATIC IMPORTED)
set_property(TARGET libyang PROPERTY IMPORTED_LOCATION ${libyang_archive})
add_dependencies(libyang project_libyang)

add_custom_command(
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/project_libyang/objs
    COMMAND cmake -E make_directory ${CMAKE_CURRENT_BINARY_DIR}/project_libyang/objs
)
set (libyang_objs ${CMAKE_CURRENT_BINARY_DIR}/project_libyang/objs)

list(APPEND libyang_inc_objs "")
foreach(yang_obj IN LISTS yang_objs)
    add_custom_command(
        OUTPUT ${libyang_objs}/${yang_obj}
        COMMAND ar -x ${libyang_archive} ${yang_obj}
        MAIN_DEPENDENCY ${libyang_objs}
        WORKING_DIRECTORY ${libyang_objs}
    )
    list(APPEND libyang_inc_objs ${libyang_objs}/${yang_obj})
endforeach(yang_obj)

include_directories(
    ${Boost_INCLUDE_DIRS}
    ${LIBYANG_INCLUDE_DIRS}
    ${LIBSSH_INCLUDE_DIRS}
    ${libnetconf_headers_location}
    ${libnetconf_location}/headers
    ${libyang_headers_location}
    ${libyang_location}/headers
)

add_library(ydk STATIC ${libydk_src} ${libnetconf_inc_objs} ${libyang_inc_objs})
target_compile_definitions(ydk PRIVATE "BOOST_ALL_DYN_LINK=1")
target_compile_definitions(ydk PRIVATE "BOOST_LOG_DYN_LINK=1")

target_link_libraries(ydk
    ${Boost_LOG_SETUP_LIBRARY}
    ${Boost_LOG_LIBRARY}
    ${BOOST_THREAD_LIBRARY}
    ${BOOST_DATE_TIME_LIBRARY}
    ${Boost_FILESYSTEM_LIBRARY}
    ${Boost_SYSTEM_LIBRARY}
    ${xml2_location}
    ${curl_location}
    ${ssh_threads_location}
    ${pcre_location}
    ${xslt_location}
    ${LIBSSH_LIBRARIES}
    ${pthread_location}
    ${dl_location}
    libnetconf
    libyang
)

set_property(TARGET ydk PROPERTY CXX_STANDARD 11)
set_property(TARGET ydk PROPERTY CXX_STANDARD_REQUIRED ON)

install(TARGETS ydk DESTINATION ${LIB_INSTALL_DIR})
install(FILES ${libydk_install_headers} DESTINATION ${INCLUDE_INSTALL_DIR})

# generate doxygen documentation for ydk_core API
find_package(Doxygen)
if(DOXYGEN_FOUND)
    set(DOXYGEN_SKIP_DOT TRUE)
    add_custom_target(doc
        COMMAND ${DOXYGEN_EXECUTABLE} ${CMAKE_BINARY_DIR}/Doxyfile
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})
    configure_file(Doxyfile.in Doxyfile)
endif()

add_subdirectory(samples)

# clean cmake cache
add_custom_target(cclean
    COMMAND make clean
    COMMAND find . -iname '*cmake*' -not -name CMakeLists.txt -not -path './CMakeModules*' -exec rm -rf {} +
    COMMAND rm -rf Makefile Doxyfile
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})

if(ENABLE_BUILD_TESTS)
    enable_testing()
    add_subdirectory(tests)
endif(ENABLE_BUILD_TESTS)
