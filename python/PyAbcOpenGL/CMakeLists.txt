##-*****************************************************************************
##
## Copyright (c) 2009-2016,
##  Sony Pictures Imageworks Inc. and
##  Industrial Light & Magic, a division of Lucasfilm Entertainment Company Ltd.
##
## All rights reserved.
##
## Redistribution and use in source and binary forms, with or without
## modification, are permitted provided that the following conditions are
## met:
## *       Redistributions of source code must retain the above copyright
## notice, this list of conditions and the following disclaimer.
## *       Redistributions in binary form must reproduce the above
## copyright notice, this list of conditions and the following disclaimer
## in the documentation and/or other materials provided with the
## distribution.
## *       Neither the name of Industrial Light & Magic nor the names of
## its contributors may be used to endorse or promote products derived
## from this software without specific prior written permission.
##
## THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
## "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
## LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
## A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
## OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
## SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
## LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
## DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
## THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
## (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
## OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
##
##-*****************************************************************************

find_package(Python2 COMPONENTS Development)

MESSAGE(STATUS "PYTHON_LIBRARIES: ${Python2_LIBRARIES}")
MESSAGE(STATUS "Boost_PYTHON_LIBRARY: ${Boost_PYTHON_LIBRARY}")
MESSAGE(STATUS "PYTHON_INCLUDE_DIR:" ${Python2_INCLUDE_DIRS})

IF (Boost_PYTHON_LIBRARY AND Python2_Development_FOUND)

  MESSAGE(STATUS "Found libboost_python: ${Boost_PYTHON_LIBRARY}")
  SET(PLIBS ${Python2_LIBRARIES} ${Boost_PYTHON_LIBRARY})

  SET(CPP_FILES
      PyAlembicOpenGL.cpp
      main.cpp
  )

  LINK_LIBRARIES(
      ${CORE_LIBS}
      ${PLIBS}
      ${PYILMBASE_PYIMATH_LIB}
  )

  INCLUDE_DIRECTORIES(${Python2_INCLUDE_DIRS} "${PROJECT_SOURCE_DIR}/lib"
      "${PROJECT_SOURCE_DIR}/python/PyAbcOpenGL")

  ADD_LIBRARY(alembicglmodule SHARED ${CPP_FILES})

  # to get rid of the "lib" prefix from cmake
  SET_TARGET_PROPERTIES(alembicglmodule PROPERTIES PREFIX "")

  INSTALL(TARGETS alembicglmodule
          LIBRARY DESTINATION lib)

ELSE()
    MESSAGE( FATAL_ERROR "Unable to find Python libs" )
ENDIF()
