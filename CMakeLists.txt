cmake_minimum_required(VERSION 3.12)

project(gumbo)

include(CheckIncludeFile)

add_library(gumbo STATIC src/attribute.c src/attribute.h src/char_ref.c src/char_ref.h src/char_ref.rl src/error.c src/error.h src/gumbo.h src/insertion_mode.h
        src/parser.c src/parser.h src/string_buffer.c src/string_buffer.h src/string_piece.c src/string_piece.h src/tag.c src/tag_enum.h src/tag_gperf.h
        src/tag_sizes.h src/tag_strings.h src/token_type.h src/tokenizer.c src/tokenizer.h src/tokenizer_states.h src/utf8.c src/utf8.h src/util.c
        src/util.h src/vector.c src/vector.h)

check_include_file("strings.h" HAVE_STRINGS_H)

if ("${HAVE_STRINGS_H}" STREQUAL "")
    target_include_directories(gumbo PRIVATE visualc/include)
endif ()

set_target_properties(gumbo PROPERTIES PUBLIC_HEADER "")

set_target_properties(gumbo PROPERTIES POSITION_INDEPENDENT_CODE ON)

INSTALL(TARGETS gumbo
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib)

INSTALL(DIRECTORY ${CMAKE_SOURCE_DIR}/src/
        DESTINATION include/gumbo
        FILES_MATCHING PATTERN "*.h")
