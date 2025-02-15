cmake_minimum_required(VERSION 3.16)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED True)

project(PseudoEngine2 VERSION 0.5.1)

add_executable(PseudoEngine2)

target_include_directories(PseudoEngine2 PRIVATE include)

if(MSVC)
    string(REPLACE "/W3" "/W4" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
else()
    target_compile_options(PseudoEngine2 PRIVATE -Wall -Wextra -pedantic)
endif()

if (CMAKE_BUILD_TYPE STREQUAL "Release")
    target_link_options(PseudoEngine2 PRIVATE -static)
endif()

target_precompile_headers(PseudoEngine2 PRIVATE include/pch.h)

target_sources(PseudoEngine2 PRIVATE 
    src/lexer/symbolLexer.cpp
    src/lexer/lexer.cpp
    src/lexer/tokens.cpp

    src/nodes/eval/arithmetic.cpp
    src/nodes/eval/comparison.cpp
    src/nodes/eval/stringcat.cpp
    src/nodes/eval/logic.cpp
    src/nodes/loop/while.cpp
    src/nodes/loop/for.cpp
    src/nodes/loop/repeatUntil.cpp
    src/nodes/loop/control.cpp
    src/nodes/functions/procedure.cpp
    src/nodes/functions/function.cpp
    src/nodes/selection/ifStatement.cpp
    src/nodes/selection/case.cpp
    src/nodes/variable/variable.cpp
    src/nodes/variable/array.cpp
    src/nodes/variable/enum.cpp
    src/nodes/variable/pointer.cpp
    src/nodes/variable/composite.cpp
    src/nodes/variable/resolver.cpp
    src/nodes/cast.cpp
    src/nodes/io/file.cpp
    src/nodes/io/io.cpp
    src/nodes/nodeResult.cpp
    src/nodes/base.cpp

    src/parser/evalExprParser.cpp
    src/parser/variableExprParser.cpp
    src/parser/arrayOperationParser.cpp
    src/parser/userType.cpp
    src/parser/procedureParser.cpp
    src/parser/functionParser.cpp
    src/parser/selectionParser.cpp
    src/parser/loopParser.cpp
    src/parser/ioParser.cpp
    src/parser/parser.cpp

    src/interpreter/types/numeric.cpp
    src/interpreter/types/boolean.cpp
    src/interpreter/types/character.cpp
    src/interpreter/types/date.cpp
    src/interpreter/types/userType.cpp
    src/interpreter/types/types.cpp
    src/interpreter/types/type_definitions.cpp
    src/interpreter/types/type_names.cpp
    src/interpreter/scope/block.cpp
    src/interpreter/scope/context.cpp
    src/interpreter/variable.cpp
    src/interpreter/array.cpp
    src/interpreter/procedure.cpp
    src/interpreter/file.cpp
    src/interpreter/builtinFunctions/string.cpp
    src/interpreter/builtinFunctions/char.cpp
    src/interpreter/builtinFunctions/numeric.cpp
    src/interpreter/builtinFunctions/date.cpp
    src/interpreter/builtinFunctions/math.cpp
    src/interpreter/error.cpp
    
    src/launch/repl.cpp
    src/launch/run.cpp

    src/main.cpp
)

install(TARGETS PseudoEngine2)

enable_testing()

function(test test_file)
    add_test(NAME ${test_file} COMMAND PseudoEngine2 ${CMAKE_CURRENT_LIST_DIR}/tests/${test_file})
endfunction()

test(selection.pseudo)
test(loop.pseudo)
test(date.pseudo)
test(arrays.pseudo)
test(procedure.pseudo)
test(function.pseudo)
test(builtin_functions.pseudo)
test(str.pseudo)
test(enum.pseudo)
test(pointer.pseudo)
test(types.pseudo)
test(files.pseudo)
