# General lib locations
SET(DEPENDENT_LIBS "")
SET(DEPENDENT_INCLUDES "")
SET(DEPENDENT_DEFS "")

# CGAL
IF(CGAL_FOUND)
    #ADD_DEFINITIONS(-DCGAL_KERNEL_NO_ASSERTIONS)
    LIST(APPEND DEPENDENT_LIBS ${CGAL_LIBRARIES} ${Boost_LIBRARIES}
        ${CGAL_3RD_PARTY_LIBRARIES} ${GMP_LIBRARIES} ${MPFR_LIBRARIES})
    LIST(APPEND DEPENDENT_INCLUDES ${CGAL_INCLUDE_DIRS}
        ${CGAL_3RD_PARTY_INCLUDE_DIRS} ${GMP_INCLUDE_DIRS} ${MPFR_INCLUDE_DIRS})
    LIST(APPEND DEPENDENT_DEFS ${CGAL_3RD_PARTY_DEFINITIONS} ${CGAL_DEFINITIONS})
    IF (CGAL_HEADER_ONLY)
        # It seems CGAL does not capture this flag in ${CGAL_DEFINITIONS}...
        LIST(APPEND DEPENDENT_DEFS -DCGAL_HEADER_ONLY)
    ENDIF (CGAL_HEADER_ONLY)
ENDIF(CGAL_FOUND)

# Source files
FILE(GLOB SRC_FILES *.cpp)
FILE(GLOB INC_FILES *.h)

# Include this directory
INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR})

ADD_LIBRARY(cgal_wrapper SHARED ${SRC_FILES} ${INC_FILES})
TARGET_INCLUDE_DIRECTORIES(cgal_wrapper SYSTEM PUBLIC ${DEPENDENT_INCLUDES})
TARGET_LINK_LIBRARIES(cgal_wrapper Mesh ${DEPENDENT_LIBS})
TARGET_COMPILE_DEFINITIONS(cgal_wrapper PUBLIC ${DEPENDENT_DEFS})

ADD_CUSTOM_TARGET(cgal DEPENDS cgal_wrapper)
