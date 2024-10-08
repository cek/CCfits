PROJECT(CCfits)
CMAKE_MINIMUM_REQUIRED(VERSION 3.1)

set (CMAKE_CXX_STANDARD_REQUIRED on)
set (CMAKE_CXX_STANDARD 11)

# Allow the developer to select whether to build Dynamic or Static libraries
OPTION (BUILD_SHARED_LIBS "Build Shared Libraries" OFF)
OPTION (USE_DLL_RUNTIME "Use MD/MDd runtime options" ON)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}")
set (LIB_DESTINATION "${CMAKE_INSTALL_PREFIX}/lib${LIB_SUFFIX}")
set (BIN_DESTINATION "${CMAKE_INSTALL_PREFIX}/bin")
set (INCLUDE_INSTALL_DIR "${CMAKE_INSTALL_PREFIX}/include/CCfits")

# Define project version
SET(${PROJECT_NAME}_MAJOR_VERSION 2)
SET(${PROJECT_NAME}_MINOR_VERSION 6)
SET(${PROJECT_NAME}_VERSION ${${PROJECT_NAME}_MAJOR_VERSION}.${${PROJECT_NAME}_MINOR_VERSION})
SET(LIB_NAME CCfits)

# Define
IF(MSVC)
    ADD_DEFINITIONS(-D_CRT_SECURE_NO_DEPRECATE)
    IF(USE_DLL_RUNTIME)
        ADD_COMPILE_OPTIONS($<$<CONFIG:>:/MD>
                            $<$<CONFIG:Debug>:/MDd>
                            $<$<CONFIG:Release>:/MD>
                            $<$<CONFIG:RelWithDebInfo>:/MD>)
    ELSE()
        ADD_COMPILE_OPTIONS($<$<CONFIG:>:/MT>
                            $<$<CONFIG:Debug>:/MTd>
                            $<$<CONFIG:Release>:/MT>
                            $<$<CONFIG:RelWithDebInfo>:/MT>)
    ENDIF()
ENDIF()

#add_subdirectory (src)

#find_package(CFITSIO REQUIRED)
INCLUDE_DIRECTORIES(${CFITSIO_INCLUDE_DIR})

SET (LIB_TYPE STATIC)
IF (BUILD_SHARED_LIBS)
  SET (LIB_TYPE SHARED)
ENDIF (BUILD_SHARED_LIBS)

FILE(GLOB H_FILES "*.h")
SET(H_FILES ${H_FILES} CCfits)

SET(SRC_FILES
    AsciiTable.cxx
    BinTable.cxx
    ColumnCreator.cxx
    Column.cxx
    ColumnData.cxx
    ColumnVectorData.cxx
    ExtHDU.cxx
    FITS.cxx
    FitsError.cxx
    FITSUtil.cxx
    GroupTable.cxx
    HDUCreator.cxx
    HDU.cxx
    KeyData.cxx
    KeywordCreator.cxx
    Keyword.cxx
    PHDU.cxx
    Table.cxx
)

ADD_LIBRARY(${LIB_NAME} ${LIB_TYPE} ${H_FILES} ${SRC_FILES})
TARGET_LINK_LIBRARIES(${LIB_NAME} ${CFITSIO_LIBRARY}
)

SET_TARGET_PROPERTIES(${LIB_NAME} PROPERTIES
    VERSION ${${PROJECT_NAME}_VERSION}
)
install(TARGETS ${LIB_NAME} DESTINATION ${LIB_DESTINATION})
install(FILES ${H_FILES} DESTINATION ${INCLUDE_INSTALL_DIR})

ENABLE_TESTING()

#SET(TEST_FILES file1.pha)
#FILE(COPY ${TEST_FILES} DESTINATION ${CCfits_BINARY_DIR})
#ADD_EXECUTABLE(cookbook cookbook.cxx)
INCLUDE_DIRECTORIES(${CMAKE_SOURCE_DIR} "${CMAKE_SOURCE_DIR}/../")
#TARGET_LINK_LIBRARIES(cookbook ${LIB_NAME} ${CFITSIO_LIBRARY})
#ADD_TEST(cookbook cookbook)
