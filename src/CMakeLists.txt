set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin")

set ( CMAKE_C_FLAGS "-fPIC")
#find_package(Lua REQUIRED)
find_package(Lua "5.2" EXACT)

if("${LUA_VERSION_STRING}" VERSION_LESS "5.2.0")
	message(FATAL_ERROR "Lua 5.2.0 or later is required, but found ${LUA_VERSION_STRING}.")
endif()

if(UNIX AND NOT APPLE)
        set ( DESTINATION "/usr/local")
	find_library(DL_LIBRARY dl)
	list(APPEND LUA_LIBRARIES ${DL_LIBRARY})
endif()

if(MSYS)
        set ( DESTINATION "c:/usr/local")
        if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
                  set(CMAKE_INSTALL_PREFIX "c:/usr/local" CACHE PATH "..." FORCE)
        endif()
endif()

include_directories(
	"${PROJECT_SOURCE_DIR}/include"
	"${LUA_INCLUDE_DIR}"	
)


add_subdirectory(lib)
add_subdirectory(bin)
add_subdirectory(tests)
