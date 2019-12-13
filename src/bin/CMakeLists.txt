add_executable(${bootstrap}
	tolua.c
	toluabind_default.c
)

if(UNIX AND NOT APPLE)
	find_library(DL_LIBRARY dl)
	list(APPEND LUA_LIBRARIES ${DL_LIBRARY})
endif()

target_link_libraries(${bootstrap}
        ${libname}
	"${LUA_LIBRARIES}"
)


file(GLOB LUA_FILES
	RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}"
	"lua/*.lua"
)

add_custom_command(OUTPUT toluabind.c
	COMMAND ${bootstrap} -C -o "${CMAKE_CURRENT_BINARY_DIR}/toluabind.c" tolua.pkg
	DEPENDS ${bootstrap} tolua.pkg ${LUA_FILES}
	WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}"
)

add_executable(${progname}
	tolua.c
	toluabind.c
)

target_link_libraries(${progname}
        ${libname}
	"${LUA_LIBRARIES}"
)


install(TARGETS ${progname}
                RUNTIME DESTINATION bin
                COMPONENT ${progname}
)
