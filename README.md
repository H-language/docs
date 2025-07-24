# Hydrogen Language Documentation

## a quick overview

H is a single-file syntactic layer that transforms C into a more readable, functional programming language.

H language provides:
- Intuitive keywords replacing outdated terms
- Functional programming patterns
- Cross-platform abstractions
- Zero overhead bloat
- Single header file implementation
- Unified file and folder IO
- Modern tools, functions, and data-structures

## Installation

Simply include `H.h` in your C project:

```c
#include "H.h"
```

Then compile with GCC (MinGW), or TCC.

## Core Language Transforms

### Pointer and Reference Operations

| C Syntax | H Syntax | Description |
|----------|----------|-------------|
| `type*` | `type ref` | Pointer type |
| `&var` | `ref_of( var )` | Address-of operator |
| `*ptr` | `val_of( ptr )` | Dereference operator |
| `(type)val` | `to( type, val )` | Type cast |
| `*(type*)&val` | `cast( type, val )` | Reinterpret cast |

### Boolean and Logic Operations

| C Syntax | H Syntax | Description |
|----------|----------|-------------|
| `!` | `not` | Logical NOT |
| `&&` | `and` | Logical AND |
| `\|\|` | `or` | Logical OR |
| `^` | `xor` | Logical XOR |
| `%` | `mod` | Modulo |
| `==` | `is` | Equality |
| `!=` | `isnt` | Inequality |
| `x ? A : B` | `pick( x, A, B )` | Ternary operator |

### Special Values

```c
null	// (void*)0
yes		// 1 (true)
no		// 0 (false)
```

## Type System

If the type is unknown, it's:
```
anon	// void
```

### Number Types

Natural (cannot be less than zero), Integer (can be negative), Rational (has a fractional part).

| Type | Size | Range |
|------|------|-------|
| `n1` | 1 byte | 0 to 255 (unsigned char) |
| `i1` | 1 byte | -128 to 127 (signed char) |
| `n2` | 2 bytes | 0 to 65,535 (unsigned short) |
| `i2` | 2 bytes | -32,768 to 32,767 (signed short) |
| `n4` | 4 bytes | 0 to 4,294,967,295 (unsigned int) |
| `i4` | 4 bytes | -2,147,483,648 to 2,147,483,647 (signed int) |
| `r4` | 4 bytes | -inf to inf |
| `n8` | 8 bytes | 0 to 18,446,744,073,709,551,615 (unsigned long long) |
| `i8` | 8 bytes | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 (signed long long) |
| `r8` | 8 bytes | -inf to inf |

### Other Types

```c
byte        // char
flag        // _Bool
```

## Control Flow

### Loops

```c
loop                    // for(;;) - infinite loop
while(condition)        // standard while
until(condition)        // while(not(condition))
next                    // continue
skip                    // break
jump label              // goto label
```

### Iteration Helpers

```c
// Range iteration: for( i = from; i <= to; i += step )
range_step( i, from, to, step )
range( i, from, to )              // step = 1

// Index iteration: for( i = 0; i < size; i += step )
iter_step( i, size, step )
iter( i, size )                   // step = 1

// 2D grid iteration
iter_grid( x, y, width, height )

// Repeat n times
repeat( n )
```

### Switch Statements

```c
with( val ) // jump to a when() depending on what it is
{
    when( 1, 2, 3 )
    {
    	// code
    	skip;
    }
    
    when( 4 )
    {
    	// code
    } // there's no skip, so it continues:
    
    other
    {
    	// this code is ran if val isn't 1, 2, or 3. But will if it's 4 etc.
    }
}
// skip takes us out to here
```

### Conditional Helpers

```c
if_null(ptr)            // if( ptr is null )
if_not_null(ptr)        // if( ptr isnt null )
if_any(a, b, c)         // if( a or b or c )
if_all(a, b, c)         // if( a and b and c )
if_none(a, b, c)        // if( not( a or b or c) )
skip_if(condition);      // if( condition ) skip;
next_if(condition);      // if( condition ) next;
out_if(condition) val;      // if( condition ) out val;
```

## Functions

### Function Declaration

```c
fn function_name(params)    // static inline void function_name(params)
{
    out value;              // return value;
}
```

### Storage Classes

```c
temp variable       // register variable
perm variable       // static variable
global variable     // explicitly global
embed function      // static inline function
```

## Memory and String Operations

### Memory Operations

```c
bytes_copy(from, size, to)              // memcpy
bytes_copy_internal(from, size, to)     // memmove
bytes_fill(ptr, value, size)            // memset
bytes_clear(ptr, size)                  // memset(ptr, 0, size)
bytes_compare(a, b, size)               // memcmp
```

### String Operations

```c
bytes_paste(from, to)                   // strcpy
bytes_measure(str)                      // strlen
bytes_end(str)                          // str[0] = '\0'
```

### Advanced Copy Operations

```c
bytes_copy_move(from, size, to_ref)     // Copy and advance destination pointer
bytes_paste_move(from, to_ref)          // Copy string and advance pointer
bytes_set_move(byte, to_ref)            // Set byte and advance pointer
```

## Math Operations

```c
MIN(a, b)               // Minimum of two values
MAX(a, b)               // Maximum of two values
MIN3/MIN4               // Minimum of 3/4 values
MAX3/MAX4               // Maximum of 3/4 values
MEAN(a, b)              // Average of two values
CLAMP(v, min, max)      // Constrain value to range
ABS(v)                  // Absolute value
SIGN(v)                 // Sign of value (1 or -1)
SQR(v)                  // Square (v * v)
CUBE(v)                 // Cube (v * v * v)
SNAP(v, multiple)       // Snap to multiple
```

## Structures and Objects

### Basic Struct

```c
struct(Point)
{
    r4 x, y;
};

// Usage:
Point p = make(Point, 10.0, 20.0);
```

### Objects with Methods

```c
object(Entity)
{
    i4 id;
    byte name[32];
};

object_fn(Entity, update)
{
    // 'this' is automatically available as Entity ref
    this->id++;
}

// Usage:
Entity e = {0};
call(&e, update);       // Calls update if not null
```

### Fusion and Group

```c
fusion(value)           // union Value
{
    i4 integer;
    r4 rational;
};

group(Color, n1)        // enum with underlying type
{
    red,
    green,
    blue
};
```

## File I/O

### File Operations

```c
// Open files
file f = open_file_loading("path.txt");
file f = open_file_saving("output.txt");

// Load/Save
byte buffer[1024];
file_load(&f, buffer);
file_save(&f, data, size);

// Memory mapped files
file f = map_file("large.dat", path_size);
// Access via f.mapped_bytes
file_unmap(&f);

// Cleanup
file_close(&f);
file_delete("path.txt");
```

### File Utilities

```c
flag file_exists(path)
flag folder_exists(path)
n8 get_file_size(path)
create_folder(path)
```

### Directory Operations

```c
byte entries[100][PATH_MAX_SIZE];
n2 count;

// Get all entries
count = get_entries_in_folder(dir, entries, 100, entry_any);

// Get only files
count = get_files(dir, entries, 100);

// Get only folders  
count = get_folders(dir, entries, 100);
```

### Path Operations

```c
path("folder", "subfolder", "file.txt")    // "folder/subfolder/file.txt"
path_up_folder(path_buffer)                // Remove last path component
byte ref exe_path = get_exe_path()         // Get executable path
```

## Terminal Operations

### Basic Output

```c
print("Hello, World!")              // Output string
print_size(buffer, 100)             // Output specific size
print_newline()                     // Output newline
```

### Formatted Output

```c
print_format("Value: <c:g>%d</c:g>\n", 42);

// Format tags:
// <c:COLOR>   - Set color (r,g,b,c,m,y,w)
// <c:d>       - Default color
// <bg:COLOR>  - Background color
// <b>         - Bold
// <i>         - Italic
// <u>         - Underline
// </TAG>      - Disable formatting
// <>          - Insert string argument
// </>         - Insert path separator
```

### Format Bytes Function

```c
// Format to buffer with size tracking
byte buffer[256];
n4 size = format_bytes(buffer, "Name: <c:b><></c:b>", "John");

// Format and advance pointer
byte ref ptr = buffer;
format_bytes_move(ptr, "Value: <c:g>%d</c:g>\n", 42);
```

### Terminal Colors

```c
terminal_set_color(red)
terminal_set_bg_color(blue)
terminal_set_bold()
terminal_reset_format()
terminal_clear()
```

### Terminal Input

```c
byte ref input = get_terminal_input();
```

## Variadic Functions

```c
fn foo( const byte ref format, ... )
{
    arg_list args;
    args_init(args, format);
    
    i4 num = args_next(args, i4);
    byte ref str = args_next(args, ref);
    
    args_end(args);
}
```

## Platform Abstraction

H automatically detects and adapts to the platform:

```c
OS_LINUX        // 1 if Linux, 0 otherwise
OS_WINDOWS      // 1 if Windows, 0 otherwise
OS_MACOS        // 1 if macOS, 0 otherwise
OS_NAME         // String: "Linux", "Windows", "macOS"
SEPARATOR       // "/" on Unix, "\\" on Windows
```

## Memory Size Constants

```c
KB( n )           // n * 1,000
MB( n )           // n * 1,000,000
GB( n )           // n * 1,000,000,000
KiB( n )          // n * 1,024
MiB( n )          // n * 1,048,576
GiB( n )          // n * 1,073,741,824
```

## Utility Macros

### Default Arguments

```c
#define DEFAULT( default_value, args... ) 
// Returns args if provided, otherwise default_value

#define DEFAULTS( ( defaults_tuple ), args... )
// used like:
#define foo( name, args... ) name( DEFAULTS( ( 1, 2, 3 ), args ) )
// foo( test ) -> test( 1, 2, 3 )
// foo( test, 7 ) -> test( 7, 2, 3 )
// foo( test, 7, 8 ) -> test( 7, 8, 3 )
// foo( test, 7, 8, 9 ) -> test( 7, 8, 9 )
// foo( test, 7,, 9 ) -> test( 7,, 9 ) !! it doesn't skip
```

### Compile-Time Helpers

```c
COUNT_ARGS( a, b, c )     // Returns 3
STRINGIFY( hello )        // Converts to "hello"
CHAIN( L, R, mid, args... )  // L arg1 R mid L arg2 R
```

### One-Time Execution

```c
once
{
    // This code runs only once, even in loops
    print( "loading complete\n" );
}
```

### Byte Array Declaration

```c
declare_bytes( buffer, KB( 1 ) );           // Creates an empty buffer[1000], and a buffer_ref that's at [0]
declare_bytes( msg, 32, "Hello" );         // Creates msg[32] = "Hello", and a msg_ref that's after the 'o'
msg[0] = '7'; // msg is now "7ello"
msg_ref[0] = 'E'; // msg is now "7elloE"
```

### Character Classification

```c
is_letter( byte )         // Check if alphabetic
is_number( byte )         // Check if numeric
```

## Entry Point

```c
start
{
    out executable_success;     // return 0
    out executable_failure;     // return 1
    out executable_warning;     // return 2
}
```

## Example Programs

### Hello World

```c
#include "H.h"

start
{
    print( "Hello, World!\n" );
    out executable_success;
}
```

### File Processing

```c
#include "H.h"

start
{
    file f = open_file_loading( "input.txt" );
    if_null( f.handle )
    {
        print( "Failed to open file\n" );
        out executable_failure;
    }
    
    byte buffer[ KB( 10 ) ];
    file_load( ref_of( f ), buffer );
    
    print_format( "File contents: <>\n", buffer );
    
    file_close( ref_of( f ) );
    out executable_success;
}
```

## License

H is released under CC0 (Creative Commons Zero) - effectively public domain. FOSS forever.
