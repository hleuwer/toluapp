add_library(${libname} SHARED
	tolua_event.c
	tolua_is.c
	tolua_map.c
	tolua_push.c
	tolua_to.c
)

target_link_libraries(
        ${libname}
        "${LUA_LIBRARIES}"
)

set_target_properties(${libname} PROPERTIES OUTPUT_NAME "tolua++${luavers}")

install(TARGETS ${libname}
                LIBRARY DESTINATION lib
                COMPONENT ${libname})
#install(TARGETS ${libname}
#                RUNTIME DESTINATION bin
#                COMPONENT ${libname})
