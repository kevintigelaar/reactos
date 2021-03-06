
remove_definitions(-DWINVER=0x502)
add_definitions(-DWINVER=0x600)

add_definitions(
    -D__WINESRC__
    -D_WINE
    -Dclose=_close)

include_directories(BEFORE ${REACTOS_SOURCE_DIR}/sdk/include/reactos/wine
                    ${REACTOS_SOURCE_DIR}/sdk/include/reactos/libs/zlib)

spec2def(wininet.dll wininet.spec ADD_IMPORTLIB)

list(APPEND SOURCE
    cookie.c
    dialogs.c
    ftp.c
    gopher.c
    http.c
    internet.c
    netconnection.c
    urlcache.c
    utility.c
    precomp.h)

add_library(wininet MODULE
    ${SOURCE}
    rsrc.rc
    ${CMAKE_CURRENT_BINARY_DIR}/wininet_stubs.c
    ${CMAKE_CURRENT_BINARY_DIR}/wininet.def)

set_module_type(wininet win32dll)
target_link_libraries(wininet wine ${PSEH_LIB} zlib)
add_delay_importlibs(wininet secur32 crypt32 cryptui)
add_importlibs(wininet mpr shlwapi shell32 user32 advapi32 ws2_32 normaliz kernel32_vista msvcrt kernel32 ntdll)
add_pch(wininet precomp.h SOURCE)
add_cd_file(TARGET wininet DESTINATION reactos/system32 FOR all)
