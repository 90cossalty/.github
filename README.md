## Contents:
- [C Coding Standards](#c-coding-standards)
    - [Syntax](#syntax)
        - [Miscellaneous](#miscellaneous)
        - [Variable Names and Declarations](#variable-names-and-declarations)
        - [Structs and Typedefs](#structs-and-typedefs)
        - [Functions](#c-functions)
        - [Control Flow](#c-control-flow)
        - [Macros](#macros)
        - [Comments](#c-comments)
        - [Braces](#braces)
        - [Header Files](#header-files)
    - [Guidelines](#guidelines)
        - [Safe Function Versions (Required)](#safe-function-versions)
        - [Heap Alloc Macros](#heap-alloc-macros)
        - [Avoid Multiple Pointers](#avoid-multiple-pointers)
        - [Single Return Point and Variable](#single-return-point-and-variable)
        - [Justify Commented Code (Required)](#justify-commented-code)
        - [Avoid Duplicating Data (Required)](#avoid-duplicating-data)
        - [Init + Deinit Functions](#init-and-deinit-functions)
        - [Doxygen (Required)](#doxygen)
        - [Guard Clauses (Required)](#guard-clauses)
- [Python Coding Standards](#functions)
    - [Variables](#python-variables)
    - [Functions](#python-functions)
    - [Classes](#classes)
    - [Explicit Typing](#explicit-typing)
    - [Comments](#python-comments)
    - [Imports](#imports)
    - [Grouping](#grouping)

# C Coding Standards

## Syntax

### Miscellaneous

1. Files should be tabbed using 4 spaces == 1 tab
1. Generally aim for neat, readable code. Refactors taking working code and making it architecturally sound will be required before tickets are merged
1. Typecasts must be made explicit

### Variable Names and Declarations

1. All lower case, underscores between words
1. Variables of the same type *may* be declared on the same line
1. For pointers, the `*` must be placed closest to the type name, with no space
1. Use the *const* keyword where possible (eg. as applicable in function parameters)
1. Constants should be declared in all caps, with underscores in between the names
1. Variables should be declared at the top of the function
1. Variable types should be the Windows variant where possible.
1. Use `TYPE*` rather than `PTYPE`, except for `STR` variants
1. Initialize with correct value (eg `NULL`, `INVALID_HANDLE_VALUE`)

**Example: Variable Names/Declarations**
```C
#define A_CONSTANT 3.1415926535
INT a_variable_name,
another_variable_name;
CHAR some_variable;
BYTE* a_pointer = NULL;
HANDLE a_handle = INVALID_HANDLE_VALUE;
```
---
- Variables will be placed in comment groups
  - Cleanup
  - Locals

Cleanup should contain only variables hat this function cleans up; caller-cleanup variables would be included in locals.

**Example: Comment Groups**
```c
// Cleanup
CHAR* heap_string = NULL;

// Locals
INT a = 0;
DOUBLE b = 0.0;
```

### Structs and Typedefs

1. Structs  
    a. Names and declarations should be all uppercase with underscores in between words  
    b. The private struct identifier should start with an underscore  
    c. For pointers, the `*` will be placed closest to the type name (rather than the variable name, with no space in between the type and `*`)  
    d. Braces will conform to the [Brace Convention](#braces) section

1. Typedef/Function Pointers
    a. Name will be all uppercase with NO underscores (to visually differentiate them from structure types)
    b. The function pointer name will have "PROC" appended to the end to denote that it is a function pointer
    c. Follows all other [function rules](#c-functions)

**Example: Struct and Function Pointer**
```c
typedef struct _MY_STRUCT
{
    INT a;
    CHAR b;
} MY_STRUCT

typedef INT (WINAPI *ADDNUMBERSPROC)(INT a, INT b);
```

### C Functions

1. Names/Declarations
    a. PascalCase (camelCase with first letter capitalized)
    b. No space between function name and parenthesis
    c. One space between comma and following parameter
    d. Use const for parameters where possible
    e. For pointers, the `*` will be placed closest to the type name (rather than the variable name, with no space between the the type name and `*`)
    f. Parameters *may* be split up over multiple lines if long enough - in this case parameters must be one per line, and the start of each declaration will be aligned with each other
    g. Braces must conform to the [Brace Conventions](#braces) section
    h. Private functions must be denoted with a leading underscore

1. Callers check return values
1. Use "cleanup" label(s) on all functions (except for wrapper functions that do nothing except call another function and return its value
1. Functions require SAL annotation and must be should be checked in review

**Example: C Functions**
```c
VOID MyFunc1(const INT* my_int, CHAR* my_string)
{
    // code goes here
cleanup:
    return;
}

// Generally auto-formatting will split these up as necessary
CHAR* MyFunc2(INT value,
              INT another_value,
              CHAR some_other_variable);

VOID PrivateFunc (const INT* my_int, CHAR* my_string)
{
    // Code here
cleanup:
    return;
}
```

### C Control Flow

1. Parentheses must be padded with one space
1. Compound assignment-comparisons inside if-statements are NOT allowed
1. A multi-line comment with a summary an done line per comparison should be used to explain complex boolean expressions
1. Ternaries are not allowed (exception for [macros](#macros)

**Example: C Control Flow**
```c
// Good practice
some_int = strcmp(some_string, "hello");
if ( some_int == 0 )
{
    // Code goes here
}

// Bad practice
if ( ( some_int = strcmp(some_string, "hello") ) == 0 )
{
    // Code goes here
}

/*
 * This comparison finds out if y is double x
 * x is used in division and cannot be 0
 * y should be double x to proceed
 */

if ( x != 0 && y / x == 2 )
{
    // Code goes here
}

for ( INT i = 0; i < 10; i++ )
{
    // Code goes here
}
```

### Macros

1. Names should be all uppercase with an underscore between words
1. Variables inside macros should use augmented Hungarian Notation
    a. function pointers must be indicated by fp prefix (eg `fpThing`)
    b. generic type indicated in macros must be indicated by T
    c. `size_t` will be represented by the st prefix

**Example: Macros**
```c
#define DO_FUNC(fpThing) ...
#define VEC_THING(T, stNumThings) ...
```

### C Comments

1. Single-line comments must use `//` as their start, and have a single space between the `//` and the comment text
    a. Do not use `/* single line comment */` as this makes it difficult to block-comment code
    b. Exception may be made for macros, where this is the only way to viably comment a single line in multi-line macros

**Example: C Comments**
```c
// A single-line comment

/*
 * A multi
 * line
 * comment
 */

INT my_int = 0; // An inline comment
```

### Braces

1. Braces must be placed on their own line with no indentation relative to the scope that they are declaring
1. One-line statements without braces are not allowed

```c
VOID MyFunc(void)
{
    // Code goes here
}
```

### Header Files

1. Keep includes in the code file and put in header file only when necessary
1. Headers contain (except when it makes more sense to have something local in the C file)
    - Required include statements
    - Macros
    - typedefs
    - function declarations
1. Top of the file should have `#pragma once` followed by the include statements with everything else after that
1. Private/helper function not included in header file

```c
#pragma once
#include <stdlib>
#define PI 3.14159

typedef ULONG error;

VOID MyFunc();
```

## Guidelines

### Safe Function Versions
**REQUIRED**  

Use safe versions of functions, if not used a detailed comment as to why should be written and reviewed. Use functions such as memcpy_s instead of memcpy, strcpy_s instead of strcpy, etc.

**Example: Safe Functions**
```c
CHAR buff_a [4];
CHAR buff_b = "A long string here";

// strcpy(buff_b, buff_a); // Allows buffer overflow!
strcpy_s(buff_a, 4, buff_b); // Only writes to the size passed here
```
### Heap Alloc Macros
Help prevent heap memory errors by encapsulating memory calls with macros

**Example: Alloc Macros**
```c
// Used for allocating a SINGLE instance of any type on the heap
#define MALLOC_SINGLE(T) (T*)calloc(1, sizeof(T))

// Used for allocating an array of any type on the heap
#define MALLOC_ARRAY(T, stNum) (T*)calloc(stNum, sizeof(T))

/*
 * Generally, use HFREE instead, if you know that
 * the null-check is unnecessary, then
 * this is cleaner
 */
#define HFREE_UNCHECKED(pVal) \
    free(pVal); \
    (pVal) = NULL;

// Used for freeing heap memory
#define HFREE(pVal) \
    if ( pVal != NULL ) \
    { \
        HFREE_UNCHECKED(pVal); \
    }
```

### Avoid Multiple Pointers

This is to prevent excess memory allocation calls that could potentially cause errors and make it more difficult to read. Heap allocation is necessary but question if before accepting it as necessary. Initialization by passing double pointers to functions is rarely necessary.

```c
CHAR ** buffer = NULL; // BAD PRACTICE
/*
 * Also be wary of indirection in pointers resulting in
 * double pointers in single pointer variable
 */

VOID MyFunc(MyStruct ** pointer_to_init) // BAD PRACTICE; RARELY NECESSARY
{
    // Code goes here
}
```

### Single Return Point and Variable

With the use of `cleanup` label(s), it is best practice to only have one return point at the end of the cleanup section.

There should also only be one return variable. This keeps the program simpler while preventing desyncs of the return variables.

The use of separate variables to store "local" return codes may be necessary in some cases, however question if it is necessary before merge. Often, it is an indicator of bad design. 

```c
INT MyFunc(INT a)
{
    INT ret = 0; // Only one return value
    INT local_ret = 0; // Bad Practice

    // Bad Practice
    if ( a == 0 )
    {
        ret = 0;
        return ret;
    }

    // Good Practice
    if ( a == 0 )
    {
        ret = 0;
        goto cleanup;
    }

cleanup:
    return 0;
}
```

### Justify Commented Code

**REQUIRED**

Code commented and left in should have a comment explaining the importance and relevance to the project. The code itself should be commented using single-line comments. This is because this allows the use of VSCode auto-comment (Ctrl+/) IDE automatic block comment features typically do not support our block comment style; and this style has the additional benefit of providing clear distinction between comment and code.

```c
/*
 * This is removed because important_check is invalid currently
 * but needs to be added after update x
 */
// if ( important check )
// {
//     ImportantFunc()
// }
```

### Avoid Duplicating Data

**REQUIRED**

Examples:

1. Passing structs to functions by value (eg. not using a pointer to them) should be avoided, as it causes the entire structure to be copied)

1. Assigning structs to a new struct instances (`MyStruct a = b` where and by are both `MyStruct` instances) should be avoided where possible as it copies the data.

If this needs to occur, it should be explicitly documented why via comments in the code

```c
SPECIAL_STRUCT my_struct;
// Bad practice, copies the whole structure:
InitSpecialStruct(my_struct);
// Good practice, sends the address of the stuct to pass as reference:
InitSpecialStruct(&my_struct);
```

### Init and Deinit Functions

We should take single pointers, assume that the memory is already initialized, and simply initalize or deinitalize any internal structure.

```c
RaguError VectorInit(Vector* vec, SIZE_T initial_cap)
{
    RaguError ret = RAGU_SUCCESS;
    vec->size = 0;
    vec->capacity = INITIAL_CAPACITY;
    vec->data = MALLOC_ARRAY(char, INITIAL_CAPACITY);
    if ( vec->data == NULL )
    {
        ret = RAGU_OUT_OF_MEMORY;
        goto cleanup;
    }

cleanup:
    return ret;
}
```

### Doxygen
**REQUIRED**
1. Doxygen documentation must use `/* */` with each line beginning with a `*` (see example)
1. Each Doxygen command must start with a @ and be followed by a space
1. File documentation
    - Required commands
        - File
        - Brief
1. Function pointer
    - Required commands
        - Typedef - name
    - A short description on the line following the command
1. Macro
    - Required commands
        - Brief - a brief description
        - Param - the parameter description should start with the type needed then description
1. Struct
    - Required commands
        - brief - a brief description of the struct
        - Comments above each struct member containing a description of the field's purpose
1. Function
    - Required commands
        - Brief - One line description of the struct
        - Params [in, out, in/out] - the description of the parameter. One of these commands should exist per parameter. None are necessary for functions that take no parameters
            - `[in]` indicates that a value is consistent. I.e it is not changed by the function being called but instead is only used as input
            - `[out]` indicates that a value is directly reassigned by the function being called, ie. via a double pointer (which is generally bad practice)
            -  `[in/out]` indicates that a parameter is taken then modified
        - return: the type of the return value, and a description of what it represents. Not required if the return type is void
1. Additional Optional Commands
    - warning: a warning about the function
    - note: a note about the function
    - todo: Things to be done in the future
    - retval: the description of possible return values

```c
/**
 * @file filename.c
 *
 * @brief a brief description of file contents
 *
 * @details An in-depth analysis of that the file has in it
 *
 */

/**
 * @typedef ADDNUMBERSPROC
 * This is the description of the function pointer
 */
typedef INT (WINAPI* ADDNUMBERSPROC)(INT a, INT b);

/**
 * @brief If the expression is true, then log something
 * 
 * @param[in] pExprCheck   BOOL - The expression to check
 * @param[in] logger       MACRO - The logger to be used
 * @param[in] ...          EXPR - the printf expression printed by the logger
 */
#define LOG_IF_TRUE(pExprCheck, logger, ...) \
    if (pExprCheck)                          \
    {                                        \
        logger(__VA_ARGS__);                 \
    }

/**
 * @brief Information about the structure
 */
typedef struct _MY_STRUCT
{
    /** Used in var1 instances */
    INT var1;
    /** Used in var2 instances */
    CHAR var2;
} MY_STRUCT;

/**
 * @brief Adds a and b together, determines whether answer is even
 * 
 * @param[in,out] a - Number to be added, holds the result of the addition
 * @param[in] b - Second number to be added
 *
 * @return BOOL  - whether the value is even
 * @retval TRUE  - the answer is even
 * @retval FALSE - the answer is odd
 */
BOOL MyFunc(INT* a, INT b)
{
    // Code goes here
}
```
### Guard Clauses
**REQUIRED**

Guard clauses should be used when possible. This helps prevent excess work being done prior to returning. This also helps the code be presented in a neater fashion avoiding nested `if ... else` statements

```c
INT MyFunc(INT a, INT b)
{
    if ( b == 0 )
    {
        goto cleanup;
    }

    if ( a == 1 )
    {
        goto cleanup;
    }

cleanup:
    return a;
}
```

# Python Coding Standards

1. Python coding standards will conform to the expectations of PEP 257, with the changes enforced by our pylint configuration (we need a copy of that for gitlab)
1. pylint *must* be run on Python code prior to merge request and must produce 0 anomilies
1. Generally aim for neat, readable code. Refactors taking working code and making it architecturally sound will be required before tickets are merged.
1. Don't use python keywords as variable names
1. Use error dictionary names for error checks, not the numeric code

### Python Variables

1. All lower case, underscores between words
1. Variables of the same type *may* be declared on the same line;
1. Initialize with correct values (None, 0, etc)

```py
def some_func() -> None:
    fun_variable = ""
```

### Python Functions

1. Names/Declarations
    - Lower case with underscores between words
    - No space between function names and parenthesis
    - One space between comma and following parameter
    - Follows grouping syntax rules
    - One space padding around the arrow to return type
    - Private functions denoted with leading underscore
1. Exceptions for error cases are preferable to error codes
1. if a function returns a value, callers must check it

```py
def my_func() -> None:
    # Code goes here

def long_func(arg1: int,
              arg2: int,
              really_long_arg_name: ProjectClass
             ) -> ProjectClass:
    # Code goes here
``` 

### Classes

1. Names/Declarations
    - `PascalCase` (Camel case with first letter capitalized)
    - No space between class name and parenthesis

```py
class TestClass(ParentClass):
    # Function stuff

class ParentClass:
    # Function stuff
```

### Explicit Typing

1. Function arguments
1. Function returns including init functions but excluding inherited functions
1. Project objects in functions

```py
def my_func(self, name: Person, number_pets: int) -> None:
    pets: PetsList = name.Pets
    # Function stuff
```

### Python Comments

1. Single line comments:
    - Use the `#` for single-line comments
    - Use a space after the `#`
1. Multi-line comments:
    - Use three single quotes `'''` on both sides
    - The comment begins on the line following the quotes

Note: Docstrings are special, required, comments directly below what they describe. They use three double quotes on the same line as the docstring, if possible

```py
# this is a single-line comment
'''
This is a multi-line comment
It is split across multiple lines
like this.
'''

def some_func():
    """This is a single-line docstring for a nop function"""
    pass

def some_other_func():
    """
    This is a multi-line docstring
    required for when a function is more complicated and needs a
    much longer docstring to document fully.

    pass is a null operation - when it is executed, nothing happens.
    It is particularly useful as a placeholder when a statement is
    required syntactically but no code needs to be executed
    """
    pass
```

### Imports

1. Imports to go on separate lines
1. Imports need a blank line between the three groups below
1. Imports are broken into 3 groups:
   1. Core Python
   1. Dependencies
   1. Project

```py
# Core Python
import os

# Dependencies
from project.dependencies import Dependency
from project.dependencies import OtherDependency
# No comma-separated imports!

from project.foo import bar
```

### Grouping
Groups of 3 or more of arguments/variables should be put 1 per line indented to the same level with the bracket/parenthesis on the line after the last element.

```py
def my_func() -> None:
    # Important code
    other_func(arg1,
               arg2,
               really_long_arg_name,
               some_arg
               )
```
