SET(RSPAMADMSRC rspamadm.c
        commands.c
        pw.c
        configtest.c
        fuzzy_convert.c
        configdump.c
        control.c
        confighelp.c
        stat_convert.c
        signtool.c
        lua_repl.c
        ${CMAKE_BINARY_DIR}/src/workers.c
        #${CMAKE_BINARY_DIR}/src/modules.c - defined in rspamdserver
        ${CMAKE_SOURCE_DIR}/src/controller.c
        ${CMAKE_SOURCE_DIR}/src/fuzzy_storage.c
        ${CMAKE_SOURCE_DIR}/src/worker.c
        ${CMAKE_SOURCE_DIR}/src/rspamd_proxy.c)
INCLUDE_DIRECTORIES(${CMAKE_CURRENT_BINARY_DIR})
IF (ENABLE_HYPERSCAN MATCHES "ON")
    LIST(APPEND RSPAMADMSRC "${CMAKE_SOURCE_DIR}/src/hs_helper.c")
ENDIF()
ADD_EXECUTABLE(rspamadm ${RSPAMADMSRC})
TARGET_LINK_LIBRARIES(rspamadm rspamd-server)

IF (NOT NO_TARGET_VERSIONS)
    SET_TARGET_PROPERTIES(rspamadm PROPERTIES VERSION ${RSPAMD_VERSION})
ENDIF ()

SET_TARGET_PROPERTIES(rspamadm PROPERTIES LINKER_LANGUAGE CXX)
ADD_BACKWARD(rspamadm)
INSTALL(TARGETS rspamadm RUNTIME DESTINATION bin)
