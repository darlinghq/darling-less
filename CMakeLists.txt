project(less)

cmake_minimum_required(VERSION 2.4.0)

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

include(darling_exe)
add_definitions(-DTARGET_OS_MAC=1)
add_definitions(-D__APPLE__ -D__DYNAMIC__)
# add_definitions(-DSUPPORT_UTMPX)

# Hide warnings
add_definitions(
	-Wno-incompatible-pointer-types-discards-qualifiers
)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -U_POSIX_C_SOURCE -D__DARWIN_UNIX03 -nostdinc -fblocks -fPIC -ggdb -O0")

include_directories(${CMAKE_SOURCE_DIR}/src/libc/include/FreeBSD)
include_directories(${CMAKE_SOURCE_DIR}/src/libc/include/NetBSD)
include_directories(${CMAKE_SOURCE_DIR}/src/libc/gen)
include_directories(${CMAKE_SOURCE_DIR}/src/libc/darwin)
include_directories(${CMAKE_SOURCE_DIR}/src/libc/stdtime/FreeBSD)
include_directories(${CMAKE_SOURCE_DIR}/src/launchd/liblaunch)
include_directories(${CMAKE_SOURCE_DIR}/src/external/libdispatch)
include_directories(${CMAKE_SOURCE_DIR}/src/external/zlib)
include_directories(${CMAKE_SOURCE_DIR}/src/external/bzip2)
include_directories(${CMAKE_SOURCE_DIR}/src/ncurses/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR})

include(InstallSymlink)
set(BINARY_PACKAGING_MODE ON)

set(less_sources
	less/brac.c
	less/charset.c
	less/ch.c
	less/cmdbuf.c
	less/command.c
	less/decode.c
	less/edit.c
	less/filename.c
	less/forwback.c
	less/help.c
	less/ifile.c
	less/input.c
	less/jump.c
	#less/lessecho.c
	less/line.c
	less/linenum.c
	less/lsystem.c
	less/main.c
	less/mark.c
	#less/mkhelp.c
	less/optfunc.c
	less/option.c
	less/opttbl.c
	less/os.c
	less/output.c
	less/position.c
	less/prompt.c
	#less/regexp.c
	less/screen.c
	#less/scrsize.c
	less/search.c
	less/signal.c
	less/tags.c
	less/ttyin.c
	less/version.c

	)

add_darling_executable(less ${less_sources})
add_darling_executable(lessecho less/lessecho.c)

target_link_libraries(less ncurses)

InstallSymlink(less libexec/darling/usr/bin/more)
install(TARGETS less lessecho
	DESTINATION libexec/darling/usr/bin)

install(FILES less.1 more.1 lessecho.1 DESTINATION libexec/darling/usr/share/man/man1)
