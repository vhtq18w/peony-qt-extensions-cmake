cmake_minimum_required(VERSION 3.0.0)

project(peony-qt-admin VERSION 0.1 LANGUAGES CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)

find_package(Qt5 COMPONENTS Core Gui Widgets Concurrent REQUIRED)
find_package(PkgConfig)

pkg_search_module(PEONY_QT REQUIRED peony-qt)
message(${PEONY_QT_INCLUDE_DIRS})
message(${PEONY_QT_LIBRARIES})

# Configurable paths
set(PKEXEC_PATH "/usr/bin/pkexec" CACHE FILEPATH "pkexec program path.")
set(PEONY_PATH "/usr/bin/peony-qt" CACHE FILEPATH "peony program path.")
set(PLUMA_PATH "/usr/bin/pluma" CACHE FILEPATH "pluma program path.")
set(TERMINAL_PATH "/usr/bin/mate-terminal" CACHE FILEPATH "mate-terminal program path.")

configure_file(org.freedesktop.peony-qt-admin.policy.in
               ${CMAKE_CURRENT_SOURCE_DIR}/org.freedesktop.peony-qt-admin.policy)

include_directories(${PEONY_QT_INCLUDE_DIRS})

add_library(${PROJECT_NAME} SHARED "admin-menu-plugin.cpp")

target_link_libraries(${PROJECT_NAME} Qt5::Core Qt5::Gui Qt5::Widgets Qt5::Concurrent ${PEONY_QT_LIBRARIES})

install(TARGETS peony-qt-admin DESTINATION /usr/lib/peony-qt-extensions)
install(FILES org.freedesktop.peony-qt-admin.policy DESTINATION /usr/share/polkit-1/actions)
