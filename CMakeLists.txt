cmake_minimum_required(VERSION 3.14)

project(livox_sdk2)

set(CMAKE_CXX_STANDARD 11)

message(STATUS "main project dir: " ${PROJECT_SOURCE_DIR})

if (CMAKE_CROSSCOMPILING)
	set(THREADS_PTHREAD_ARG
		"PLEASE_FILL_OUT-FAILED_TO_RUN"
		CACHE STRING "Result from TRY_RUN" FORCE)
endif()

if (UNIX)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pthread")
endif(UNIX)

add_subdirectory(sdk_core)
add_subdirectory(samples)

find_package(ament_cmake REQUIRED)

# インストール設定 (ライブラリやヘッダーを適切な場所に配置)
install(
  DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/
  DESTINATION include/livox_sdk2
)

# ライブラリ本体(.so)を install/lib にコピーする設定
install(TARGETS livox_lidar_sdk_shared
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

# 自身を ament パッケージとして登録
ament_package()
