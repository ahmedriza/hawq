CMAKE_MINIMUM_REQUIRED(VERSION 2.9)

SET(TEST_WORKING_DIR ${CMAKE_CURRENT_SOURCE_DIR}/data/)
# ADD_DEFINITIONS(-DGTEST_LANG_CXX11=0 -Dprivate=public -Dprotected=public -DDATA_DIR="${TEST_WORKING_DIR}/")
ADD_DEFINITIONS(-DGTEST_LANG_CXX11=0 -DDATA_DIR="${TEST_WORKING_DIR}/")

ADD_SUBDIRECTORY(function)
ADD_SUBDIRECTORY(unit)
ADD_SUBDIRECTORY(secure)

IF(TEST_RUNNER)
    SEPARATE_ARGUMENTS(TEST_RUNNER_LIST UNIX_COMMAND ${TEST_RUNNER})
ENDIF(TEST_RUNNER)

ADD_CUSTOM_TARGET(unittest
	COMMAND ${TEST_RUNNER_LIST} ${CMAKE_CURRENT_BINARY_DIR}/unit/unit
	DEPENDS unit
	WORKING_DIRECTORY ${TEST_WORKING_DIR}
	COMMENT "Run Unit Test..."
)

ADD_CUSTOM_TARGET(functiontest
	COMMAND ${TEST_RUNNER_LIST} ${CMAKE_CURRENT_BINARY_DIR}/function/function
	DEPENDS function
	WORKING_DIRECTORY ${TEST_WORKING_DIR}
	COMMENT "Run Function Test..."
)

ADD_CUSTOM_TARGET(securetest
	COMMAND ${TEST_RUNNER_LIST} ${CMAKE_CURRENT_BINARY_DIR}/secure/secure
	DEPENDS secure
	WORKING_DIRECTORY ${TEST_WORKING_DIR}
	COMMENT "Run Security Function Test..."
)

ADD_CUSTOM_TARGET(test 
    COMMAND ${CMAKE_MAKE_PROGRAM} unittest || true
    COMMAND ${CMAKE_MAKE_PROGRAM} functiontest || true
    COMMENT "Run All Test..."
)

SET(unit_SOURCES ${unit_SOURCES} PARENT_SCOPE)
SET(function_SOURCES ${function_SOURCES} PARENT_SCOPE)
SET(secure_SOURCES ${secure_SOURCES} PARENT_SCOPE)
