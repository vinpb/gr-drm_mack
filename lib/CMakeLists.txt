# Copyright 2011,2012,2016 Free Software Foundation, Inc.
#
# This file was generated by gr_modtool, a tool from the GNU Radio framework
# This file is a part of gr-drm_receiver
#
# GNU Radio is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 3, or (at your option)
# any later version.
#
# GNU Radio is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with GNU Radio; see the file COPYING.  If not, write to
# the Free Software Foundation, Inc., 51 Franklin Street,
# Boston, MA 02110-1301, USA.

########################################################################
# Setup library
########################################################################
include(GrPlatform) #define LIB_SUFFIX

include_directories(${Boost_INCLUDE_DIR})
link_directories(${Boost_LIBRARY_DIRS})

list(APPEND drm_receiver_sources
    cell_demapping_impl.cc
)

set(drm_receiver_sources "${drm_receiver_sources}" PARENT_SCOPE)
if(NOT drm_receiver_sources)
	MESSAGE(STATUS "No C++ sources... skipping lib/")
	return()
endif(NOT drm_receiver_sources)

add_library(gnuradio-drm_receiver SHARED ${drm_receiver_sources})
target_link_libraries(gnuradio-drm_receiver ${Boost_LIBRARIES} ${GNURADIO_ALL_LIBRARIES})
set_target_properties(gnuradio-drm_receiver PROPERTIES DEFINE_SYMBOL "gnuradio_drm_receiver_EXPORTS")

if(APPLE)
    set_target_properties(gnuradio-drm_receiver PROPERTIES
        INSTALL_NAME_DIR "${CMAKE_INSTALL_PREFIX}/lib"
    )
endif(APPLE)

########################################################################
# Install built library files
########################################################################
include(GrMiscUtils)
GR_LIBRARY_FOO(gnuradio-drm_receiver RUNTIME_COMPONENT "drm_receiver_runtime" DEVEL_COMPONENT "drm_receiver_devel")

########################################################################
# Build and register unit test
########################################################################
include(GrTest)

include_directories(${CPPUNIT_INCLUDE_DIRS})

list(APPEND test_drm_receiver_sources
    ${CMAKE_CURRENT_SOURCE_DIR}/test_drm_receiver.cc
    ${CMAKE_CURRENT_SOURCE_DIR}/qa_drm_receiver.cc
)

add_executable(test-drm_receiver ${test_drm_receiver_sources})

target_link_libraries(
  test-drm_receiver
  ${GNURADIO_RUNTIME_LIBRARIES}
  ${Boost_LIBRARIES}
  ${CPPUNIT_LIBRARIES}
  gnuradio-drm_receiver
)

GR_ADD_TEST(test_drm_receiver test-drm_receiver)

########################################################################
# Print summary
########################################################################
message(STATUS "Using install prefix: ${CMAKE_INSTALL_PREFIX}")
message(STATUS "Building for version: ${VERSION} / ${LIBVER}")

