#
# SPDX-License-Identifier: BSD-2-Clause
#
############################################
#
# How to build bsdunzip
#
############################################
IF(ENABLE_UNZIP)

  SET(bsdunzip_SOURCES
    bsdunzip.c
    bsdunzip.h
    bsdunzip_platform.h
    cmdline.c
    la_getline.c
    la_getline.h
    la_queue.h
    ../libarchive_fe/err.c
    ../libarchive_fe/err.h
    ../libarchive_fe/lafe_platform.h
    ../libarchive_fe/passphrase.c
    ../libarchive_fe/passphrase.h
  )
  INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR}/../libarchive_fe)

  # bsdunzip documentation
  SET(bsdunzip_MANS bsdunzip.1)

  # How to build bsdunzip
  ADD_EXECUTABLE(bsdunzip ${bsdunzip_SOURCES})
  IF(CMAKE_SYSTEM_NAME STREQUAL "iOS")
    SET_TARGET_PROPERTIES(bsdunzip PROPERTIES MACOSX_BUNDLE FALSE)
  ENDIF()
  IF(ENABLE_UNZIP_SHARED)
    TARGET_LINK_LIBRARIES(bsdunzip archive ${ADDITIONAL_LIBS})
  ELSE(ENABLE_UNZIP_SHARED)
    TARGET_LINK_LIBRARIES(bsdunzip archive_static ${ADDITIONAL_LIBS})
    SET_TARGET_PROPERTIES(bsdunzip PROPERTIES COMPILE_DEFINITIONS
                                 LIBARCHIVE_STATIC)
  ENDIF(ENABLE_UNZIP_SHARED)

  # Installation rules
  IF(CMAKE_SYSTEM_NAME STREQUAL "iOS")
    INSTALL(TARGETS bsdunzip BUNDLE DESTINATION bin RUNTIME DESTINATION bin)
  ELSE()
    INSTALL(TARGETS bsdunzip RUNTIME DESTINATION bin)
  ENDIF()
  INSTALL_MAN(${bsdunzip_MANS})
ENDIF(ENABLE_UNZIP)

add_subdirectory(test)
