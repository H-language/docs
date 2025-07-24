# <glow>H</glow>ydrogen <glow>Lang</glow>uage Documentation

## Contents
- [Installation](#installation)
- [Core Language Transforms](#core-language-transforms)
  - [Pointer and Reference Operations](#pointer-and-reference-operations)
  - [Boolean and Logic Operations](#boolean-and-logic-operations)
- [Type System](#type-system)
  - [Number Types](#number-types)
  - [Other Types](#other-types)
- [Control Flow](#control-flow)
  - [Loops](#loops)
  - [Iteration Helpers](#iteration-helpers)
  - [Switch Statements](#switch-statements)
  - [Conditional Helpers](#conditional-helpers)
- [Functions](#functions)
  - [Function Declaration](#function-declaration)
  - [Storage Classes](#storage-classes)
- [Memory and String Operations](#memory-and-string-operations)
  - [Memory Operations](#memory-operations)
  - [String Operations](#string-operations)
  - [Advanced Copy Operations](#advanced-copy-operations)
- [Math Operations](#math-operations)
- [Structures and Objects](#structures-and-objects)
  - [Basic Struct](#basic-struct)
  - [Objects with Methods](#objects-with-methods)
  - [Fusion and Group](#fusion-and-group)
- [File I/O](#file-io)
  - [File Operations](#file-operations)
  - [File Utilities](#file-utilities)
  - [Directory Operations](#directory-operations)
  - [Path Operations](#path-operations)
- [Terminal Operations](#terminal-operations)
  - [Basic Output](#basic-output)
  - [Formatted Output](#formatted-output)
  - [Format Bytes Function](#format-bytes-function)
  - [Terminal Colors](#terminal-colors)
  - [Terminal Input](#terminal-input)
- [Variadic Functions](#variadic-functions)
- [Platform Abstraction](#platform-abstraction)
- [Memory Size Constants](#memory-size-constants)
- [Utility Macros](#utility-macros)
  - [Default Arguments](#default-arguments)
  - [Compile-Time Helpers](#compile-time-helpers)
  - [One-Time Execution](#one-time-execution)
  - [Byte Array Declaration](#byte-array-declaration)
  - [Character Classification](#character-classification)
- [Entry Point](#entry-point)
- [Example Programs](#example-programs)
  - [Hello World](#hello-world)
  - [File Processing](#file-processing)
- [License](#license)

## a quick overview

<glow>H</glow> is a single-file syntactic layer that transforms C into a more readable, functional programming language.

Hydrogen Language provides:
- Intuitive and explicit keywords
- Functional programming patterns
- Cross-platform abstractions
- Zero overhead bloat
- Single header file implementation
- Unified file and folder IO
- Modern functions and data-structures

## Installation

Simply include `H.h` in your C project:

<pre>
<M>#include</M> <Y>"</Y><C>H.h</C><Y>"</Y>
</pre>

Then compile with <b>GCC</b> (MinGW), or <glow>TCC</glow>.

## Core Language Transforms

### Pointer and Reference Operations

| H Syntax | Definition |
|----------|-------------|
| <G>TYPE</G> <glow>ref</glow> | Reference to type |
| <glow>ref_of(</glow> <G>VAR</G> <glow>)</glow> | Reference-of variable |
| <glow>val_of(</glow> <G>REF</G> <glow>)</glow> | Value of reference |
| <glow>to(</glow> <G>TYPE</G><glow>,</glow> <G>VAL</G> <glow>)</glow> | Type cast |
| <glow>cast(</glow> <G>TYPE</G><glow>,</glow> <G>VAR</G> <glow>)</glow> | Reinterpret cast |

<pre>
<glow>null</glow> <G>// a zero-ref</G>
</pre>

### Boolean and Logic Operations

| H Syntax | Definition |
|----------|-------------|
| `not` | Logical NOT |
| `and` | Logical AND |
| `or` | Logical OR |
| `xor` | Logical XOR |
| `mod` | Modulo |
| `is` | Equality |
| `isnt` | Inequality |
| `pick( x, A, B )` | Ternary operator |

## Type System

If the type is unknown, it's:
```
anon
// anon ref x = ref_of( to( anon, y ) ); !! Cannot get ref from type-cast
// anon ref x = ref_of( cast( anon, y ) ); ?? You can get the ref from an reinterpret-cast
// anon ref x = to( anon ref, ref_of( y ) ); It's better to get the ref, then cast the ref
```

### Number Types

Natural (cannot be less than zero), Integer (can be negative), Rational (has a fractional part).

| Type | Bytes | Range |
|------|------|-------|
| `n1` | <b>1</b> | <b>0</b> <i>to</i> <b>255</b> |
| `i1` | <b>1</b> | <b>-128</b> <i>to</i> <b>127</b> |
| `n2` | <b>2</b> | <b>0</b> <i>to</i> <b>65,535</b> |
| `i2` | <b>2</b> | <b>-32,768</b> <i>to</i> <b>32,767</b> |
| `n4` | <b>4</b> | <b>0</b> <i>to</i> <b>4,294,967,295</b> |
| `i4` | <b>4</b> | <b>-2,147,483,648</b> <i>to</i><br><b>2,147,483,647</b> |
| `r4` | <b>4</b> | <b>-inf</b> <i>to</i> <b>inf</b> |
| `n8` | <b>8</b> | <b>0</b> <i>to</i><br><b>18,446,744,073,709,551,615</b> |
| `i8` | <b>8</b> | <b>-9,223,372,036,854,775,808</b> <i>to</i><br><b>9,223,372,036,854,775,807</b> |
| `r8` | <b>8</b> | <b>-inf</b> <i>to</i> <b>inf</b> |

### Other Types

```c
byte // equivalent to an i1, used for explicit byte handling
flag // yes or no
```
Values:
```
yes	// 1
no	// 0
```

## Control Flow

### Loops

```c
loop			// for(;;) - infinite loop
while( condition )	// standard while
until( condition )	// while(not(condition))
next			// continue
skip			// break
jump label		// goto label
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
 
