cmake_minimum_required(VERSION 2.6)
project (hamcast_libtunnel_module)

# Set up environment paths to cmake modules and libhamcast
set (CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR})

if (CMAKE_BUILD_TYPE STREQUAL Debug)
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -stdlib=libc++ -std=c++11 -DHC_ENABLE_LOGGING -g -Wextra -Wall -pedantic")
else ()
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -stdlib=libc++ -std=c++11 -O3")
endif (CMAKE_BUILD_TYPE STREQUAL Debug)

#set (CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})

set (LIBTUNNELMODULE_SRC    src/tunnel_message.cpp
                            src/tunnel.cpp
                            src/module.cpp)

# search for libs
find_package (LibHamcast REQUIRED)

include_directories ( ./include ${HAMCAST_INCLUDE} )

add_library(libtunnelmodule SHARED ${LIBTUNNELMODULE_SRC})

target_link_libraries (libtunnelmodule ${CMAKE_LD_LIBS} ${HAMCAST_LIBRARY})

# install lib
set (LIBTUNNELMODULE_VERSION_MAJOR 0)
set (LIBTUNNELMODULE_VERSION_MINOR 15)
set (LIBTUNNELMODULE_VERSION_PATCH 1)
set(LIBRARY_VERSION ${LIBTUNNELMODULE_VERSION_MAJOR}.${LIBTUNNELMODULE_VERSION_MINOR}.${LIBTUNNELMODULE_VERSION_PATCH})
set(LIBRARY_SOVERSION ${LIBTUNNELMODULE_VERSION_MAJOR})
set_target_properties(libtunnelmodule PROPERTIES
                        SOVERSION ${LIBRARY_SOVERSION}
                        VERSION ${LIBRARY_VERSION}
                        OUTPUT_NAME tunnelmodule)
