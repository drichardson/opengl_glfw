cmake_minimum_required(VERSION 3.4)

add_subdirectory(glfw)

include_directories(glfw/include)

add_executable(howdy WIN32 MACOSX_BUNDLE howdy.c)

target_link_libraries(howdy glfw ${GLFW_LIBRARIES})

set(WINDOWS_BINARIES howdy)

if (MSVC)
    # Tell MSVC to use main instead of WinMain for Windows subsystem executables
    set_target_properties(${WINDOWS_BINARIES} PROPERTIES
                          LINK_FLAGS "/ENTRY:mainCRTStartup")
endif()

if (APPLE)
    set_target_properties(howdy PROPERTIES MACOSX_BUNDLE_BUNDLE_NAME "Simple")
    set_target_properties(${WINDOWS_BINARIES} PROPERTIES
                          MACOSX_BUNDLE_SHORT_VERSION_STRING ${GLFW_VERSION}
                          MACOSX_BUNDLE_LONG_VERSION_STRING ${GLFW_VERSION_FULL}
                          MACOSX_BUNDLE_INFO_PLIST "${GLFW_SOURCE_DIR}/CMake/MacOSXBundleInfo.plist.in")
endif()

