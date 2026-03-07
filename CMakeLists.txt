cmake_minimum_required(VERSION 3.14)
project(llm-parse CXX)
set(CMAKE_CXX_STANDARD 17)

foreach(EXAMPLE html_parse markdown_parse chunker text_stats)
    add_executable(${EXAMPLE} examples/${EXAMPLE}.cpp)
    target_include_directories(${EXAMPLE} PRIVATE include)
    target_compile_options(${EXAMPLE} PRIVATE
        $<$<CXX_COMPILER_ID:MSVC>:/W4>
        $<$<NOT:$<CXX_COMPILER_ID:MSVC>>:-Wall -Wextra>
    )
endforeach()
