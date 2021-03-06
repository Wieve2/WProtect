cmake_minimum_required(VERSION 2.8.3)

If(BUILD_HOST STREQUAL "x86_32")
    Set (${CMAKE_SYSTEM_PROCESSOR} i386)
    Message("Host X86_32")
endif()

project(testproject)
# Find includes in corresponding build directories
set(CMAKE_INCLUDE_CURRENT_DIR ON)


If (WIN32)
Set(QT_PATH "D:/Qt/5.5/mingw492_32/lib/") 
Set(Qt5Core_DIR ${QT_PATH}"/cmake/Qt5Core")
Set(Qt5Gui_DIR ${QT_PATH}"/cmake/Qt5Gui")
Set(Qt5Widgets_DIR ${QT_PATH}"/cmake/Qt5Widgets")
endif()

If (UNIX)
Set(QT_PATH "/usr/lib32")
Set(Qt5Core_DIR "/usr/lib32/cmake/Qt5Core")
Set(Qt5Gui_DIR "/usr/lib32/cmake/Qt5Gui")
Set(Qt5Widgets_DIR "/usr/lib32/cmake/Qt5Widgets")
endif()  

link_directories(${QT_PATH}"/plugins/")
link_directories(${QT_PATH}"/plugins/imageformats") 
link_directories(${QT_PATH}"/plugins/platforminputcontexts")
link_directories(${QT_PATH}"/plugins/platformthemes")
link_directories(${QT_PATH}) 
#
find_package(Qt5Widgets PATHS ${QT_PATH}"/cmake")
if (Qt5Widgets_FOUND)
    include_directories(${Qt5Widgets_INCLUDE_DIRS})
    add_definitions(${Qt5Widgets_DEFINITIONS})
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${Qt5Widgets_EXECUTABLE_COMPILE_FLAGS}")
    If (WIN32)
     Set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -mwindows")
    Endif()
    Message("Modules Paths : ${Qt5Widgets_DIR}")
    message("Qt Include : ${Qt5Widgets_INCLUDE_DIRS}")
    message("Qt Libraries :  ${Qt5Widgets_LIBRARIES}")
    Message("CMake Cur: ${CMAKE_CURRENT_LIST_DIR}")
    Message("${Qt5${_module_dep}_DIR}")
endif()
find_package(Qt5Core PATHS ${QT_PATH}"/cmake") 
qt5_wrap_ui(UI_FILES WProtectDialog.ui)
set(MOC_SRCS)
qt5_wrap_cpp(MOC_SRCS WProtectGui.h)
add_executable(WProtectGui main.cc WProtectGui.cc ${MOC_SRCS}  ${UI_FILES})
#set_target_properties(WProtectGui PROPERTIES COMPILE_FLAGS "-m32" LINK_FLAGS "-m32")
#qt5_use_modules(WProtectGui Widgets)

target_link_libraries(WProtectGui Qt5::Widgets Qt5::Core ${LIBS})
