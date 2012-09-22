######################################################################################
# Packages needed for examples
######################################################################################

set(CMAKE_C_FLAGS "-Wall")

if (WIN32)
  set_source_files_properties(glview.c PROPERTIES LANGUAGE CXX)

  set(THREADS_USE_PTHREADS_WIN32 true)
  find_package(Threads REQUIRED)

  include_directories(${THREADS_PTHREADS_INCLUDE_DIR})
endif()

add_executable(glview glview.c)
add_executable(glview_xtion_record glview_xtion_record.c)
add_executable(xtion_register xtion_register.c)
if(BUILD_AUDIO)
  add_executable(wavrecord wavrecord.c)
  add_executable(micview micview.c)
endif()

# Mac just has everything already
if(APPLE)
  set(CMAKE_EXE_LINKER_FLAGS "-framework OpenGL -framework GLUT")
  target_link_libraries(glview freenect)
  target_link_libraries(glview_xtion_record freenect)
  target_link_libraries(xtion_register freenect)
# Linux, not so much
else()

  find_package(Threads REQUIRED)
  find_package(OpenGL REQUIRED)
  find_package(GLUT REQUIRED)

  include_directories(${OPENGL_INCLUDE_DIRS} ${GLUT_INCLUDE_DIR} ${USB_INCLUDE_DIRS})

  target_link_libraries(glview freenect ${OPENGL_LIBRARIES} ${GLUT_LIBRARY} ${CMAKE_THREAD_LIBS_INIT} ${MATH_LIB})
  target_link_libraries(glview_xtion_record freenect ${OPENGL_LIBRARIES} ${GLUT_LIBRARY} ${CMAKE_THREAD_LIBS_INIT} ${MATH_LIB})
  target_link_libraries(xtion_register freenect ${CMAKE_THREAD_LIBS_INIT} ${MATH_LIB})
endif()


install (TARGETS glview glview_xtion_record xtion_register
  DESTINATION bin)
#install (TARGETS glview
#  DESTINATION bin)
