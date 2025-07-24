# <H>H</H>ydrogen <H>Lang</H>uage Documentation
<i><G>created by </G><EDG>ENDESGA</EDG><G> - started in 2020 - made in NZ - CC0 - FOSS forever</G></i>

-------
## a quick overview
<H>H</H> is a single-file syntactic layer that transforms C into a more readable, functional programming language.
Hydrogen Language provides:
- Intuitive and explicit keywords
- Functional programming patterns
- Cross-platform abstractions
- Zero overhead bloat
- Single header file implementation
- Unified file and folder IO
- Modern functions and data-structures

### the H mini-manifesto & some personal statements:
Why go to all the effort of making a programming language when it's not even readable to the average person? What's the point of gatekeeping "talking to computers" behind walls of unintuitive symbols? It's hard enough learning the rules, logic, workflows to program. Why make it harder with symbols and pairs of symbols that require remembering rather than simply reading? Programmers forget how difficult it was to initially understand how to code, and brush it off as "you'll learn and struggle too". But why not just bring the barrier of entry down by making a language completely readable without symbol-decryption?

My answer to this is an abstraction layer I've been crafting and honing for many years.
This is designed for younger me, who struggled to read and understand code for a very long time. The over-use of symbols in programming languages has frustrated me since the moment I started learning. Only after many years of intense study, practice, and fixation have I finally got enough knowledge to create a solution.

A lot of passion, care, and strong intention has gone into all of this.
Make what you wish existed.

-------
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
  - [with/when Statements](#with-when-statements)
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

-------
## Installation
Simply include H.h in your C project:
<pre>
<Y>#include</Y> <C>"H.h"</C>
</pre>
Then compile with <H>GCC</H>, or <H>TCC</H>.

-------
## Core Language Transforms

### Pointer and Reference Operations
| H Syntax | Definition |
|----------|-------------|
| <G>TYPE</G> <H>ref</H> | Reference to type |
| <H>ref_of(</H> <G>VAR</G> <H>)</H> | Reference-of variable |
| <H>val_of(</H> <G>REF</G> <H>)</H> | Value of reference |
| <H>to(</H> <G>TYPE</G><H>,</H> <G>VAL</G> <H>)</H> | Type cast |
| <H>cast(</H> <G>TYPE</G><H>,</H> <G>VAR</G> <H>)</H> | Reinterpret cast |
<pre>
<G>// If a ref points to no value, you use:</G>
<H>nothing</H>
<G>// byte ref x = nothing;</G>
</pre>

### Boolean and Logic Operations
| H Syntax | Definition |
|----------|-------------|
| <H>not</H> | Logical NOT |
| <H>and</H> | Logical AND |
| <H>or</H> | Logical OR |
| <H>xor</H> | Logical eXclusive-OR |
| mod | Modulo |
| is | Equality |
| isnt | Inequality |
| pick( x, A, B ) | Ternary operator |

-------
## Type System
If the type is unknown, it's:
```
anon
// anon ref x = ref_of( to( anon, y ) ); !! Cannot get ref from type-cast
// anon ref x = ref_of( cast( anon, y ) ); ?? You can get the ref from an reinterpret-cast
// anon ref x = to( anon ref, ref_of( y ) ); It's better to get the ref, then cast the ref
```

### Number Types
<H>N</H>atural <LG>(cannot be less than zero)</LG>
<H>I</H>nteger <LG>(can be negative)</LG>
<H>R</H>ational <LG>(has a fractional part)</LG>
| Type | Bytes | Range |
|------|------|-------|
| <H>n1</H> | <b>1</b> | <b>0</b> <i>to</i> <b>255</b> |
| <H>i1</H> | <b>1</b> | <b>-128</b> <i>to</i> <b>127</b> |
| <H>n2</H> | <b>2</b> | <b>0</b> <i>to</i> <b>65,535</b> |
| <H>i2</H> | <b>2</b> | <b>-32,768</b> <i>to</i> <b>32,767</b> |
| <H>n4</H> | <b>4</b> | <b>0</b> <i>to</i> <b>4,294,967,295</b> |
| <H>i4</H> | <b>4</b> | <b>-2,147,483,648</b> <i>to</i><br><b>2,147,483,647</b> |
| <H>r4</H> | <b>4</b> | <b>-inf</b> <i>to</i> <b>inf</b> |
| <H>n8</H> | <b>8</b> | <b>0</b> <i>to</i><br><b>18,446,744,073,709,551,615</b> |
| <H>i8</H> | <b>8</b> | <b>-9,223,372,036,854,775,808</b> <i>to</i><br><b>9,223,372,036,854,775,807</b> |
| <H>r8</H> | <b>8</b> | <b>-inf</b> <i>to</i> <b>inf</b> |

### Other Types
```
byte // equivalent to an i1, used for explicit byte handling
flag // yes or no
```
Values:
```
yes	// 1
no	// 0
```

-------
## Control Flow

### Loops
```
loop			// for(;;) - infinite loop
while( condition )	// standard while
until( condition )	// while(not(condition))
next			// continue
skip			// break
jump label		// goto label
```

### Iteration Helpers

```
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

### with/when Statements
<pre>
<G>// jump to a when() depending on what it is</G>
<M>with</M><Y>(</Y> val <Y>)</Y>
<C>{</C>
    <M>when</M><Y>(</Y> <C>1</C>, <C>2</C>, <C>3</C> <Y>)</Y>
    <C>{</C>
    	<G>// code</G>
    	<M>skip</M>;
    <C>}</C>
    
    <M>when</M><Y>(</Y> <C>4</C> <Y>)</Y>
    <C>{</C>
    	<G>// code</G>
    <C>}</C> <G>// there's no skip, so it continues:</G>
    
    <M>other</M>
    <C>{</C>
    	<G>// this code is ran if val
	// isn't 1, 2, or 3. But will if it's 4 etc.</G>
    <C>}</C>
<C>}</C>
<G>// skip takes us out to here</G>
</pre>

### Conditional Helpers
<pre>
<M>if_null</M><Y>(</Y>ptr<Y>)</Y>            <G>// if( ptr is null )</G>
<M>if_not_null</M><Y>(</Y>ptr<Y>)</Y>        <G>// if( ptr isnt null )</G>
<M>if_any</M><Y>(</Y>a, b, c<Y>)</Y>         <G>// if( a or b or c )</G>
<M>if_all</M><Y>(</Y>a, b, c<Y>)</Y>         <G>// if( a and b and c )</G>
<M>if_none</M><Y>(</Y>a, b, c<Y>)</Y>        <G>// if( not( a or b or c) )</G>
<M>skip_if</M><Y>(</Y>condition<Y>)</Y>;      <G>// if( condition ) skip;</G>
<M>next_if</M><Y>(</Y>condition<Y>)</Y>;      <G>// if( condition ) next;</G>
<M>out_if</M><Y>(</Y>condition<Y>)</Y> val;      <G>// if( condition ) out val;</G>
</pre>

-------
## Functions

### Function Declaration
```
fn function_name(params)    // static inline void function_name(params)
{
    out value;              // return value;
}
```

### Storage Classes

```
temp variable       // register variable
perm variable       // static variable
global variable     // explicitly global
embed function      // static inline function
```

## Memory and String Operations

### Memory Operations

```
bytes_copy(from, size, to)              // memcpy
bytes_copy_internal(from, size, to)     // memmove
bytes_fill(ptr, value, size)            // memset
bytes_clear(ptr, size)                  // memset(ptr, 0, size)
bytes_compare(a, b, size)               // memcmp
```

### String Operations

```
bytes_paste(from, to)                   // strcpy
bytes_measure(str)                      // strlen
bytes_end(str)                          // str[0] = '\0'
```

### Advanced Copy Operations

```
bytes_copy_move(from, size, to_ref)     // Copy and advance destination pointer
bytes_paste_move(from, to_ref)          // Copy string and advance pointer
bytes_set_move(byte, to_ref)            // Set byte and advance pointer
```

## Math Operations

```
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

```
struct(Point)
{
    r4 x, y;
};

// Usage:
Point p = make(Point, 10.0, 20.0);
```

### Objects with Methods

```
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

```
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

```
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

```
flag file_exists(path)
flag folder_exists(path)
n8 get_file_size(path)
create_folder(path)
```

### Directory Operations

```
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

```
path("folder", "subfolder", "file.txt")    // "folder/subfolder/file.txt"
path_up_folder(path_buffer)                // Remove last path component
byte ref exe_path = get_exe_path()         // Get executable path
```

## Terminal Operations

### Basic Output

```
print("Hello, World!")              // Output string
print_size(buffer, 100)             // Output specific size
print_newline()                     // Output newline
```

### Formatted Output

```
print_format("Value: <c:g>%d</c:g>\n", 42);

// Format tags:

```

### Format Bytes Function

```
// Format to buffer with size tracking
byte buffer[256];
n4 size = format_bytes(buffer, "Name: <c:b><></c:b>", "John");

// Format and advance pointer
byte ref ptr = buffer;
format_bytes_move(ptr, "Value: <c:g>%d</c:g>\n", 42);
```

### Terminal Colors

```
terminal_set_color(red)
terminal_set_bg_color(blue)
terminal_set_bold()
terminal_reset_format()
terminal_clear()
```

### Terminal Input

```
byte ref input = get_terminal_input();
```

## Variadic Functions

<pre>
<M>fn</M> <M>foo</M><Y>(</Y> <Y>const</Y> <Y>byte</Y> <Y>ref</Y> format, ... <Y>)</Y>
<C>{</C>
    <Y>arg_list</Y> args;
    <M>args_init</M><Y>(</Y>args, format<Y>)</Y>;
    
    <Y>i4</Y> num <Y>=</Y> <M>args_next</M><Y>(</Y>args, <Y>i4</Y><Y>)</Y>;
    <Y>byte</Y> <Y>ref</Y> str <Y>=</Y> <M>args_next</M><Y>(</Y>args, <Y>ref</Y><Y>)</Y>;
    
    <M>args_end</M><Y>(</Y>args<Y>)</Y>;
<C>}</C>
</pre>

## Platform Abstraction

H automatically detects and adapts to the platform:

```
OS_LINUX        // 1 if Linux, 0 otherwise
OS_WINDOWS      // 1 if Windows, 0 otherwise
OS_MACOS        // 1 if macOS, 0 otherwise
OS_NAME         // String: "Linux", "Windows", "macOS"
SEPARATOR       // "/" on Unix, "\\" on Windows
```

## Memory Size Constants

```
KB( n )           // n * 1,000
MB( n )           // n * 1,000,000
GB( n )           // n * 1,000,000,000
KiB( n )          // n * 1,024
MiB( n )          // n * 1,048,576
GiB( n )          // n * 1,073,741,824
```

## Utility Macros

### Default Arguments

```
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

```
COUNT_ARGS( a, b, c )     // Returns 3
STRINGIFY( hello )        // Converts to "hello"
CHAIN( L, R, mid, args... )  // L arg1 R mid L arg2 R
```

### One-Time Execution

```
once
{
    // This code runs only once, even in loops
    print( "loading complete\n" );
}
```

### Byte Array Declaration

```
declare_bytes( buffer, KB( 1 ) );           // Creates an empty buffer[1000], and a buffer_ref that's at [0]
declare_bytes( msg, 32, "Hello" );         // Creates msg[32] = "Hello", and a msg_ref that's after the 'o'
msg[0] = '7'; // msg is now "7ello"
msg_ref[0] = 'E'; // msg is now "7elloE"
```

### Character Classification

```
is_letter( byte )         // Check if alphabetic
is_number( byte )         // Check if numeric
```

## Entry Point

```
start
{
    out executable_success;     // return 0
    out executable_failure;     // return 1
    out executable_warning;     // return 2
}
```

## Example Programs

### Hello World
```
<Y>#include</Y> <C>"H.h"</C>

<M>start</M>
<C>{</C>
	<M>print</M><Y>(</Y> <C>"Hello, World!\n"</C> <Y>)</Y>;
	<M>out executable_success</M>;
<C>}</C>
```

### File Processing
```
<Y>#include</Y> <C>"H.h"</C>

<M>start</M>
<C>{</C>
	<Y>file</Y> f <Y>=</Y> <M>open_file_loading</M><Y>(</Y> <C>"input.txt"</C> <Y>)</Y>;
	<M>if_nothing</M><Y>(</Y> f.handle <Y>)</Y>
	<C>{</C>
		<M>print</M><Y>(</Y> <C>"Failed to open file\n"</C> <Y>)</Y>;
		<M>out executable_failure</M>;
	<C>}</C>
	
	<Y>byte</Y> buffer<C>[</C> <M>KB</M><Y>(</Y> <C>10</C> <Y>)</Y> <C>]</C>;
	<M>file_load</M><Y>(</Y> <M>ref_of</M><Y>(</Y> f <Y>)</Y>, buffer <Y>)</Y>;
	
	<M>print_format</M><Y>(</Y> <C>"File contents: <>\n"</C>, buffer <Y>)</Y>;
	
	<M>file_close</M><Y>(</Y> <M>ref_of</M><Y>(</Y> f <Y>) )</Y>;
	<M>out executable_success</M>;
<C>}</C>
```

## License

H is released under CC0 (Creative Commons Zero) - effectively public domain. FOSS forever.
 
