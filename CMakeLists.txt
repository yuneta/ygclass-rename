##############################################
#   CMake merde
##############################################
project(ygclass-rename C)
include(CheckIncludeFiles)
include(CheckSymbolExists)

set(CMAKE_INSTALL_PREFIX /yuneta/development/bin)

set(BIN_DEST_DIR /yuneta/development/bin)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -std=c99")

if(CMAKE_BUILD_TYPE MATCHES Debug)
  add_definitions(-DDEBUG)
  option(SHOWNOTES "Show preprocessor notes" OFF)

  if(CMAKE_COMPILER_IS_GNUCC)
    # GCC specific debug options
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O0 -g3 -ggdb3 -gdwarf-2")
    set(AVOID_VERSION -avoid-version)
  endif(CMAKE_COMPILER_IS_GNUCC)
endif(CMAKE_BUILD_TYPE MATCHES Debug)

add_definitions(-D_GNU_SOURCE)
add_definitions(-D_LARGEFILE_SOURCE -D_FILE_OFFSET_BITS=64)

check_include_files(regex.h HAVE_REGEX_H)
if(HAVE_REGEX_H)
    add_definitions(-DHAVE_REGEX_H)
else(HAVE_REGEX_H)
    message("***  WARNING: GNU C regex library not found.")
endif(HAVE_REGEX_H)

include_directories(/yuneta/development/output/include)

##############################################
#   Source
##############################################

SET (YUNO_SRCS
    ygclass_rename.c
    rename_gclass.c
    render_file.c
    /yuneta/development/yuneta/^gobj-ecosistema/ghelpers/src/00_replace_string.c
)
SET (YUNO_HDRS
)

##############################################
#   External libraries
##############################################

##############################################
#   Executable
##############################################

ADD_EXECUTABLE(ygclass-rename ${YUNO_SRCS} ${YUNO_HDRS})
TARGET_LINK_LIBRARIES(ygclass-rename
    /yuneta/development/output/lib/libjansson.a
    rt pthread dl pcre
)

##############################################
#   Installation
##############################################
install(
    TARGETS ygclass-rename
    PERMISSIONS
    OWNER_READ OWNER_WRITE OWNER_EXECUTE
    GROUP_READ GROUP_WRITE GROUP_EXECUTE
    WORLD_READ WORLD_EXECUTE
    DESTINATION ${BIN_DEST_DIR}
)

