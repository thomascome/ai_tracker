
set(AI_TRACKER_NAME "ai_tracker")

set(CMAKE_CXX_FLAGS_DEBUG "-fPIC -O0 -g -Wall")
set(CMAKE_CXX_FLAGS_RELEASE "-fPIC -O2 -Wall")

message(STATUS "source file path" ${PROJECT_SRC_ROOT})

file(GLOB_RECURSE SRC_FILE
    ../src/*.cpp
)

set(SOURCE_FILE
    ${SRC_FILE}
)

set(SNPE_INCDIR ${SNPE_SDK_BASE_DIR}/include/zdl)

add_executable(${AI_TRACKER_NAME} ${SRC_FILE})
target_compile_options(${AI_TRACKER_NAME} PUBLIC -fPIC  -O0 -g -Wall -Wnon-virtual-dtor)

find_package(Protobuf REQUIRED)
find_package(OpenCV REQUIRED )
find_package(PkgConfig REQUIRED)
pkg_check_modules(GSTREAMER REQUIRED gstreamer-1.0)
pkg_check_modules(GSTREAMER_APP REQUIRED gstreamer-app-1.0)
pkg_check_modules(JSONCPP REQUIRED jsoncpp)
pkg_check_modules(JSON REQUIRED json-glib-1.0)
pkg_check_modules(OPENCV REQUIRED opencv4)

# Header path
target_include_directories(${AI_TRACKER_NAME} PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/../inc
    ${GSTREAMER_INCLUDE_DIRS}
    ${GSTREAMER_APP_INCLUDE_DIRS}
    ${OpenCV_INCLUDE_DIRS}
    ${SNPE_INCDIR}
    ${JSON_INCLUDE_DIRS}
    ${JSONCPP_INCLUDE_DIRS}
)

target_link_libraries(${AI_TRACKER_NAME} PUBLIC 
                      pthread dl
                      opencv_core
                      opencv_imgproc
                      ${GSTREAMER_LIBRARIES}
                      ${GSTREAMER_APP_LIBRARIES}
                      ${JSON_LIBRARIES}
                      jsoncpp 
                      SNPE
                      ${Protobuf_LIBRARIES})

install(TARGETS ${AI_TRACKER_NAME} RUNTIME DESTINATION "bin")