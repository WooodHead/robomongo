cmake_minimum_required(VERSION 3.0.0)
project(Robomongo)

# Where to look first for CMake modules, before "${CMAKE_ROOT}/Modules" is checked
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake")

# Project version
set(PROJECT_VERSION_MAJOR "1")
set(PROJECT_VERSION_MINOR "4")
set(PROJECT_VERSION_PATCH "0")
set(PROJECT_VERSION_BUILD "")
set(PROJECT_VERSION "${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}.${PROJECT_VERSION_PATCH}${PROJECT_VERSION_BUILD}")

# Enable C++17 features 
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Modules
include(RobomongoPrintUtils)
include(RobomongoCMakeDefaults)
include(RobomongoDefaults)
include(RobomongoCommon)
include(RobomongoTrashSymbols)
include(RobomongoTargetArch)
include(RobomongoInstallQt)
include(RobomongoPackage)

# Search for dependencies
find_package(Threading REQUIRED)    # Wrapper arround CMake "Threads" module
find_package(Qt5Core REQUIRED)
find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)
find_package(Qt5PrintSupport REQUIRED)
find_package(Qt5Network REQUIRED)
find_package(Qt5Xml REQUIRED)
find_package(MongoDB REQUIRED)
find_package(OpenSSL REQUIRED)

if (SYSTEM_MACOSX)
    set(CMAKE_OSX_DEPLOYMENT_TARGET 10.11)
    find_package(Qt5MacExtras REQUIRED)
endif()

# todo: Use equivalent vars from googletest cmake files. Currently they are somehow not working.
set(GOOGLE_TEST_VERSION 1.8.1)
set(GOOGLE_TEST_DIR src/third-party/googletest-${GOOGLE_TEST_VERSION})

# Projects
add_subdirectory(src/third-party/libssh2)
add_subdirectory(src/robomongo/ssh)
add_subdirectory(src/third-party/qjson)
add_subdirectory(src/third-party/qscintilla)
add_subdirectory(${GOOGLE_TEST_DIR})
add_subdirectory(src/robomongo)
# todo: Enable unit tests
# add_subdirectory(src/robomongo-unit-tests) # Disabled in Robo 1.4 due to too many link errors

# Show configuration summary
include(RobomongoConfigurationSummary)