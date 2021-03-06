cmake_minimum_required(VERSION 2.6)
project(avsocks)

set(CMAKE_INSTALL_PREFIX /usr)

#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -fvisibility=hidden")


option(USE_STATIC "static link to boost" ON)

if(USE_STATIC)
	set(Boost_USE_STATIC_LIBS        ON)
	set(Boost_USE_STATIC_RUNTIME    ON)
endif()



find_package(Boost 1.47 REQUIRED system program_options regex filesystem)
find_package(OpenSSL REQUIRED)
find_package(Threads)

include_directories(${Boost_INCLUDE_DIRS})

add_executable(avsocks main.cpp sd-daemon.c)
target_link_libraries(avsocks ${Boost_LIBRARIES} ${OPENSSL_LIBRARIES} ${CMAKE_THREAD_LIBS_INIT})

install(TARGETS avsocks RUNTIME DESTINATION bin)

install(FILES avsocks.service DESTINATION lib/systemd/system)
install(FILES avsocks.socket DESTINATION lib/systemd/system)
