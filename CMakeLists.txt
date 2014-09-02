cmake_minimum_required(VERSION 2.8)

project(OculusSDK)

add_subdirectory(LibOVR)
#add_subdirectory(Samples/OculusWorldDemo)

install(FILES package.xml DESTINATION share/oculus_sdk)

