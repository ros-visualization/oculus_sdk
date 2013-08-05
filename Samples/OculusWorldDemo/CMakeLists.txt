

INCLUDE_DIRECTORIES(../../LibOVR/Include ../../LibOVR/Src)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DQT_WEBKIT -DGL_GLEXT_PROTOTYPES")

SET(SOURCE_FILES
  Player.cpp
  ../../Samples/CommonSrc/Platform/Platform.cpp
  ../../Samples/CommonSrc/Platform/Linux_Platform.cpp
  ../../Samples/CommonSrc/Platform/Linux_Gamepad.cpp
  ../../Samples/CommonSrc/Render/Render_Device.cpp
  ../../Samples/CommonSrc/Render/Render_GL_Device.cpp
  ../../Samples/CommonSrc/Render/Render_LoadTextureDDS.cpp
  ../../Samples/CommonSrc/Render/Render_LoadTextureTGA.cpp
  ../../Samples/CommonSrc/Render/Render_XmlSceneLoader.cpp
  )


add_library(OculusWorld SHARED
  ${SOURCE_FILES}
  )


add_library(tinyxml2_embedded ../../3rdParty/TinyXml/tinyxml2.cpp)

add_executable(OculusWorldDemo OculusWorldDemo.cpp)
target_link_libraries(OculusWorldDemo OculusWorld ovr tinyxml2_embedded udev pthread GL X11 Xinerama)



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