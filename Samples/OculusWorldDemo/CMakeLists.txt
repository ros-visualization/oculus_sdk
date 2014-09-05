

INCLUDE_DIRECTORIES(../../LibOVR/Include ../../LibOVR/Src)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DQT_WEBKIT -DGL_GLEXT_PROTOTYPES")

if (APPLE)
  set(PLATFORM_SOURCE_FILES
    ../../Samples/CommonSrc/Platform/OSX_Platform.mm
    ../../Samples/CommonSrc/Platform/OSX_Gamepad.cpp
  )

  find_library(OPENGL_LIBRARY OpenGL)
  set(EXTRA_LIBS ovr tinyxml2_embedded ${OPENGL_LIBRARY})
elseif (UNIX)
  set(PLATFORM_SOURCE_FILES
    ../../Samples/CommonSrc/Platform/Linux_Platform.cpp
    ../../Samples/CommonSrc/Platform/Linux_Gamepad.cpp
  )

  set(EXTRA_LIBS ovr tinyxml2_embedded udev pthread GL X11 Xinerama)
endif()

SET(SOURCE_FILES
  Player.cpp
  ../../Samples/CommonSrc/Platform/Platform.cpp
  ../../Samples/CommonSrc/Render/Render_Device.cpp
  ../../Samples/CommonSrc/Render/Render_GL_Device.cpp
  ../../Samples/CommonSrc/Render/Render_LoadTextureDDS.cpp
  ../../Samples/CommonSrc/Render/Render_LoadTextureTGA.cpp
  ../../Samples/CommonSrc/Render/Render_XmlSceneLoader.cpp
  ${PLATFORM_SOURCE_FILES}
  )


add_library(OculusWorld SHARED
  ${SOURCE_FILES}
  )
target_link_libraries(OculusWorld ${EXTRA_LIBS})


add_library(tinyxml2_embedded ../../3rdParty/TinyXml/tinyxml2.cpp)

add_executable(OculusWorldDemo OculusWorldDemo.cpp)
target_link_libraries(OculusWorldDemo OculusWorld ${EXTRA_LIBS})



#add_custom_command(
#  OUTPUT Assets.zip
#  COMMAND wget http://tarball_of_assets.zip -o Assets.zip
#)

#add_custom_command(
#  OUTPUT Assets
#  COMMAND unzip -j -d Assets Assets.zip OculusSDKAssets-linux-0.2.3/Tuscany/*
#  DEPENDS Assets.zip
#  COMMENT "Unpacking Assets for Demo"
#)

#add_custom_target( assets
#  DEPENDS Assets.zip
#)
