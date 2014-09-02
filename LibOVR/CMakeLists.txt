cmake_minimum_required(VERSION 2.8)

project(OculusSDK)

set(OculusSDK_MAJOR_VERSION 0)
set(OculusSDK_MINOR_VERSION 2)
set(OculusSDK_PATCH_VERSION 3)
set(OculusSDK_VERSION
  ${OculusSDK_MAJOR_VERSION}.${OculusSDK_MINOR_VERSION}.${OculusSDK_PATCH_VERSION})


set(INSTALL_LIB_DIR lib CACHE PATH "Installation directory for libraries")
set(INSTALL_BIN_DIR bin CACHE PATH "Installation directory for executables")
set(INSTALL_INCLUDE_DIR include CACHE PATH
  "Installation directory for header files")


if(WIN32 AND NOT CYGWIN)
  set(DEF_INSTALL_CMAKE_DIR CMake)
else()
  set(DEF_INSTALL_CMAKE_DIR lib/cmake/OculusSDK)
endif()
set(INSTALL_CMAKE_DIR ${DEF_INSTALL_CMAKE_DIR} CACHE PATH
  "Installation directory for CMake files")


include_directories(Include)

if(APPLE)
  set(PLATFORM_SOURCE_FILES
    Src/OVR_OSX_DeviceManager.cpp
    Src/OVR_OSX_HIDDevice.cpp
    Src/OVR_OSX_HMDDevice.cpp
    Src/OVR_OSX_SensorDevice.cpp
  )

  set(PLATFORM_HEADER_FILES
    Src/OVR_OSX_HIDDevice.h
    Src/OVR_OSX_DeviceManager.h
    Src/OVR_OSX_HMDDevice.h
  )

  find_library(COCOA_LIBRARY Cocoa)
  find_library(IOKIT_LIBRARY IOKit)
  set(EXTRA_LIBS ${COCOA_LIBRARY} ${IOKIT_LIBRARY})

elseif(UNIX)
  set(PLATFORM_SOURCE_FILES
    Src/OVR_Linux_DeviceManager.cpp
    Src/OVR_Linux_HIDDevice.cpp
    Src/OVR_Linux_HMDDevice.cpp
    Src/OVR_Linux_SensorDevice.cpp
  )

  set(PLATFORM_HEADER_FILES
    Src/OVR_Linux_HIDDevice.h
    Src/OVR_Linux_DeviceManager.h
    Src/OVR_Linux_HMDDevice.h
  )

  set(EXTRA_LIBS
    udev
    pthread
    GL
    X11
    Xinerama
  )
endif()

set(SOURCE_FILES
  Src/OVR_DeviceHandle.cpp
  Src/OVR_DeviceImpl.cpp
  Src/OVR_JSON.cpp
  Src/OVR_LatencyTestImpl.cpp
  Src/OVR_Profile.cpp
  Src/OVR_SensorFilter.cpp
  Src/OVR_SensorFusion.cpp
  Src/OVR_SensorImpl.cpp
  Src/OVR_ThreadCommandQueue.cpp
  Src/Util/Util_LatencyTest.cpp
  Src/Util/Util_MagCalibration.cpp
  Src/Util/Util_Render_Stereo.cpp
  Src/Kernel/OVR_Alg.cpp
  Src/Kernel/OVR_Allocator.cpp
  Src/Kernel/OVR_Atomic.cpp
  Src/Kernel/OVR_File.cpp
  Src/Kernel/OVR_FileFILE.cpp
  Src/Kernel/OVR_Log.cpp
  Src/Kernel/OVR_Math.cpp
  Src/Kernel/OVR_RefCount.cpp
  Src/Kernel/OVR_Std.cpp
  Src/Kernel/OVR_String.cpp
  Src/Kernel/OVR_String_FormatUtil.cpp
  Src/Kernel/OVR_String_PathUtil.cpp
  Src/Kernel/OVR_SysFile.cpp
  Src/Kernel/OVR_System.cpp
  Src/Kernel/OVR_ThreadsPthread.cpp
  Src/Kernel/OVR_Timer.cpp
  Src/Kernel/OVR_UTF8Util.cpp
  ${PLATFORM_SOURCE_FILES}
)

add_library(ovr SHARED ${SOURCE_FILES})

target_link_libraries(ovr ${EXTRA_LIBS})

set(INCLUDE_HEADER_FILES
  Include/OVR.h
  Include/OVRVersion.h
)

set(SRC_HEADER_FILES
  Src/OVR_SensorFilter.h
  Src/OVR_Win32_DeviceManager.h
  Src/OVR_Win32_HIDDevice.h
  Src/OVR_OSX_HMDDevice.h
  Src/OVR_Win32_HMDDevice.h
  Src/OVR_DeviceConstants.h
  Src/OVR_Profile.h
  Src/OVR_OSX_HIDDevice.h
  Src/OVR_OSX_DeviceManager.h
  Src/OVR_DeviceHandle.h
  Src/OVR_HIDDevice.h
  Src/OVR_Win32_DeviceStatus.h
  Src/OVR_HIDDeviceBase.h
  Src/OVR_JSON.h
  Src/OVR_DeviceImpl.h
  Src/OVR_HIDDeviceImpl.h
  Src/OVR_Win32_SensorDevice.h
  Src/OVR_Device.h
  Src/OVR_SensorFusion.h
  Src/OVR_ThreadCommandQueue.h
  Src/OVR_LatencyTestImpl.h
  Src/OVR_SensorImpl.h
  Src/OVR_DeviceMessages.h
  ${PLATFORM_HEADER_FILES}
)
set(KERNEL_HEADER_FILES
  Src/Kernel/OVR_Threads.h
  Src/Kernel/OVR_Types.h
  Src/Kernel/OVR_System.h
  Src/Kernel/OVR_RefCount.h
  Src/Kernel/OVR_Array.h
  Src/Kernel/OVR_StringHash.h
  Src/Kernel/OVR_Color.h
  Src/Kernel/OVR_Hash.h
  Src/Kernel/OVR_Std.h
  Src/Kernel/OVR_File.h
  Src/Kernel/OVR_Atomic.h
  Src/Kernel/OVR_String.h
  Src/Kernel/OVR_Math.h
  Src/Kernel/OVR_UTF8Util.h
  Src/Kernel/OVR_ContainerAllocator.h
  Src/Kernel/OVR_KeyCodes.h
  Src/Kernel/OVR_Timer.h
  Src/Kernel/OVR_Allocator.h
  Src/Kernel/OVR_List.h
  Src/Kernel/OVR_Alg.h
  Src/Kernel/OVR_SysFile.h
  Src/Kernel/OVR_Log.h
)
set(UTIL_HEADER_FILES
  Src/Util/Util_Render_Stereo.h
  Src/Util/Util_MagCalibration.h
  Src/Util/Util_LatencyTest.h
  )



# Needed if not shared on amd64
#SET_TARGET_PROPERTIES(ovr PROPERTIES COMPILE_FLAGS "-fPIC")

INSTALL(FILES ${INCLUDE_HEADER_FILES}
  DESTINATION include/OculusSDK/Include
)
INSTALL(FILES ${SRC_HEADER_FILES}
  DESTINATION include/OculusSDK/Src
)

INSTALL(FILES ${KERNEL_HEADER_FILES}
  DESTINATION include/OculusSDK/Src/Kernel
)
INSTALL(FILES ${UTIL_HEADER_FILES}
  DESTINATION include/OculusSDK/Src/Util
)

INSTALL(TARGETS ovr
  RUNTIME DESTINATION ${INSTALL_BIN_DIR}
  LIBRARY DESTINATION ${INSTALL_LIB_DIR}
  ARCHIVE DESTINATION ${INSTALL_LIB_DIR}
)

if (NOT APPLE)
  option(INSTALL_UDEV_RULES "Install udev rules for Oculus Rift" ON)
  IF (INSTALL_UDEV_RULES)
    INSTALL (
      FILES 90-oculus-hydro.rules
      DESTINATION "/etc/udev/rules.d"
      COMPONENT "udev"
      )
  else (INSTALL_UDEV_RULES)
    message (STATUS "Udev rules not being installed, install them with -DINSTALL_UDEV_RULES=ON")
  endif (INSTALL_UDEV_RULES)
endif()



file(RELATIVE_PATH REL_INCLUDE_DIR "${CMAKE_INSTALL_PREFIX}/${INSTALL_CMAKE_DIR}" "${CMAKE_INSTALL_PREFIX}/${INSTALL_INCLUDE_DIR}")
# ... for the build tree
set(CONF_INCLUDE_DIRS "${PROJECT_SOURCE_DIR}" "${PROJECT_BINARY_DIR}")
set(CONF_LIBRARY_DIRS "${PROJECT_BINARY_DIR}")
configure_file(OculusSDKConfig.cmake.in
  "${PROJECT_BINARY_DIR}/OculusSDKConfig.cmake" @ONLY)
# ... for the install tree
set(CONF_INCLUDE_DIRS "\${OculusSDK_CMAKE_DIR}/${REL_INCLUDE_DIR}")
set(CONF_LIBRARY_DIRS "${CMAKE_INSTALL_PREFIX}/${INSTALL_LIB_DIR}")
configure_file(OculusSDKConfig.cmake.in
  "${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/OculusSDKConfig.cmake" @ONLY)
# ... for both
configure_file(OculusSDKConfigVersion.cmake.in
  "${PROJECT_BINARY_DIR}/OculusSDKConfigVersion.cmake" @ONLY)
 
# Install the OculusSDKConfig.cmake and OculusSDKConfigVersion.cmake
install(FILES
  "${PROJECT_BINARY_DIR}/${CMAKE_FILES_DIRECTORY}/OculusSDKConfig.cmake"
  "${PROJECT_BINARY_DIR}/OculusSDKConfigVersion.cmake"
  DESTINATION "${INSTALL_CMAKE_DIR}" COMPONENT dev)
