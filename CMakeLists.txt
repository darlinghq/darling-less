project(less)

cmake_minimum_required(VERSION 3.13)

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

add_compile_definitions(
	SYSDIR="/etc"
	BINDIR="/usr/bin"
)

# Hide warnings
add_definitions(
	-Wno-incompatible-pointer-types-discards-qualifiers
)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -U_POSIX_C_SOURCE -D__DARWIN_UNIX03 -nostdinc -fblocks -fPIC -ggdb -O0")

include_directories(${CMAKE_CURRENT_SOURCE_DIR})

include(InstallSymlink)
set(BINARY_PACKAGING_MODE ON)

add_darling_executable(less
	less/brac.c
	less/ch.c
	less/charset.c
	less/cmdbuf.c
	less/command.c
	less/cvt.c
	less/decode.c
	less/edit.c
	less/filename.c
	less/forwback.c
	less/help.c
	less/ifile.c
	less/input.c
	less/jump.c
	less/line.c
	less/linenum.c
	less/lsystem.c
	less/main.c
	less/mark.c
	less/optfunc.c
	less/option.c
	less/opttbl.c
	less/os.c
	less/output.c
	less/pattern.c
	less/position.c
	less/prompt.c
	less/screen.c
	less/search.c
	less/signal.c
	less/tags.c
	less/ttyin.c
	less/version.c
)
add_darling_executable(lessecho less/lessecho.c)

target_link_libraries(less ncurses)

InstallSymlink(less libexec/darling/usr/bin/more)
install(TARGETS less lessecho
	DESTINATION libexec/darling/usr/bin)

install(FILES less.1 lessecho.1 DESTINATION libexec/darling/usr/share/man/man1)
InstallSymlink(less.1 libexec/darling/usr/share/man/man1/more.1)
