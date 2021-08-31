
cmake_minimum_required(VERSION 3.16)

project (hello-xdg-shell)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/CMake/modules")

find_package(ECM REQUIRED NO_MODULE)
list(APPEND CMAKE_MODULE_PATH "${ECM_MODULE_PATH}")

find_package(Wayland REQUIRED Client Cursor Egl)
find_package(WaylandScanner REQUIRED )
find_package(WaylandProtocols REQUIRED)

add_executable(${CMAKE_PROJECT_NAME} main.cpp )

# need to include protocol's header which is generated in the build path.
target_include_directories(${CMAKE_PROJECT_NAME} PUBLIC
	${CMAKE_CURRENT_BINARY_DIR})

target_link_libraries(${CMAKE_PROJECT_NAME} Wayland::Client Wayland::Egl
	Wayland::Cursor)

ecm_add_wayland_client_protocol(GLFW_WAYLAND_PROTOCOL_SOURCES
  PROTOCOL
  "${WaylandProtocols_DATADIR}/stable/xdg-shell/xdg-shell.xml"
  BASENAME xdg-shell)

target_sources(${CMAKE_PROJECT_NAME} PRIVATE ${GLFW_WAYLAND_PROTOCOL_SOURCES})

set_target_properties(${CMAKE_PROJECT_NAME} PROPERTIES
	CXX_STANDARD 17)

# print_target_properties(${CMAKE_PROJECT_NAME})
