# Include path
include_directories(${CMAKE_CURRENT_SOURCE_DIR})

# Dependency libraries
find_package(Threads REQUIRED)

# Add libraries
set(SPYLITE_LIB_SRC
	lcmtype_db.c
	msg_display.c
	symtab_elf.c
	timeutil.c
)
add_library(spylite STATIC ${SPYLITE_LIB_SRC})
target_link_libraries(spylite  -lpthread -ldl ${LCM_LIBRARIES} ${GLIB2_LIBRARIES})
target_include_directories(spylite PUBLIC
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
	$<INSTALL_INTERFACE:include>)
