# <H>H</H>ydrogen <H>Lang</H>uage Documentation
<i><G>created by </G><EDG>ENDESGA</EDG><G> - started in 2020 - made in NZ - CC0 - FOSS forever</G></i>

-------
## a quick overview
<H>H</H> is a single-file syntactic layer that reshapes C into a more readable and understandable language.
<H>H</H>ydrogen <H>Lang</H>uage provides:
- Intuitive and explicit keywords
- Functional programming patterns
- Cross-platform abstractions
- Zero overhead bloat
- Single header file implementation
- Unified file and folder IO
- Modern functions and data-structures

### the H mini-manifesto & some personal statements:
Why go to all the effort of making a programming language when it's not even readable to the average person? What's the point of gatekeeping "talking to computers" behind walls of unintuitive symbols? It's hard enough learning the logic, rules, and workflows to program effectively. Why make it harder with symbols and pairs of symbols that require remembering rather than simply reading?

Programmers forget how difficult it was to initially understand how to code, and brush it off as "you'll learn and struggle too". But why not just bring the barrier of entry down by making a language completely readable from the start?

My answer to this is an abstraction layer I've been crafting and honing for many years.
This is designed for younger me, who struggled to read and understand code for a very long time. The over-use of symbols in programming languages has frustrated me since the moment I started learning. Only after many years of intense study, practice, and fixation have I finally got enough knowledge to create a solution.

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
| <code><LG>TYPE</LG> <Y>ref</Y></code> | Reference to TYPE |
| <code><M>ref_of</M><Y>(</Y> <LG>VAR</LG> <Y>)</Y></code> | Reference-of VAR |
| <code><M>val_of</M><Y>(</Y> <LG>REF</LG> <Y>)</Y></code> | Value-of REF |
| <code><M>to</M><Y>(</Y> <LG>TYPE</LG>, <LG>VAL</LG> <Y>)</Y></code> | Changes VAL to TYPE |
| <code><M>cast</M><Y>(</Y> <LG>TYPE</LG>, <LG>VAR</LG> <Y>)</Y></code> | Reinterpret VAR to TYPE |

### Boolean and Logic Operations
| H Syntax | Definition |
|----------|-------------|
| <code><M>not</M></code> | Logical NOT |
| <code><M>and</M></code> | Logical AND |
| <code><M>or</M></code> | Logical OR |
| <code><M>xor</M></code> | Logical eXclusive-OR |
| <code><M>mod</M></code> | Modulo |
| <code><M>is</M></code> | Equality |
| <code><M>isnt</M></code> | Inequality |

-------
## Type System
<H>H</H> is "only bytes and references".

## a <H>byte</H> is <i>8 bits</i>
## a <H>ref</H> is <i>8 bytes</i>

If you're dealing with bytes, use:
<pre>
<Y>byte</Y>
<G>// byte x = '7';</G>
<G>// The '7' character is 55</G>
<G>// Which is 00110111 in bits</G>
</pre>

If the reference type is unknown, it's:
<pre>
<Y>anon</Y>
<G>// some_type ref y_ref = ref_of( y );</G>
<G>// anon ref x = to( anon ref, y_ref );</G>
</pre>

If the reference itself is unknown/invalid, it's:
<pre>
<C>nothing</C>
<G>// byte ref x = nothing;</G>
</pre>

### Number Types
- <H>N</H>atural <LG>(cannot be less than zero)</LG>
- <H>I</H>nteger <LG>(can be negative)</LG>
- <H>R</H>ational <LG>(has a fractional part)</LG>

<pre>
[<Y>n</Y>/<Y>i</Y>/<Y>r</Y>][<Y>size</Y>]
<G>// Where [size] is in bytes</G>
</pre>

| Type | Size | Range |
|------|------|-------|
| <code><Y>n1</Y></code> | <b>1</b> | <b>0</b> <LG><i>to</i></LG> <b>255</b> |
| <code><Y>i1</Y></code> | <b>1</b> | <b>-128</b> <LG><i>to</i></LG> <b>127</b> |
| <code><Y>n2</Y></code> | <b>2</b> | <b>0</b> <LG><i>to</i></LG> <b>65,535</b> |
| <code><Y>i2</Y></code> | <b>2</b> | <b>-32,768</b> <LG><i>to</i></LG> <b>32,767</b> |
| <code><Y>n4</Y></code> | <b>4</b> | <b>0</b> <LG><i>to</i></LG> <b>4,294,967,295</b> |
| <code><Y>i4</Y></code> | <b>4</b> | <b>-2,147,483,648</b> <LG><i>to</i></LG><br><b>2,147,483,647</b> |
| <code><Y>r4</Y></code> | <b>4</b> | <b>-inf</b> <LG><i>to</i></LG> <b>inf</b> |
| <code><Y>n8</Y></code> | <b>8</b> | <b>0</b> <LG><i>to</i></LG><br><b>18,446,744,073,709,551,615</b> |
| <code><Y>i8</Y></code> | <b>8</b> | <b>-9,223,372,036,854,775,808</b> <LG><i>to</i></LG><br><b>9,223,372,036,854,775,807</b> |
| <code><Y>r8</Y></code> | <b>8</b> | <b>-inf</b> <LG><i>to</i></LG> <b>inf</b> |

### Other Types
<pre>
<Y>flag</Y> <LG>VAR</LG>
<G>// A flag can either be:</G>
<C>yes</C> <G>// 1</G>
<G>// or</G>
<C>no</C>  <G>// 0</G>

<M>flip</M><Y>(</Y> <LG>FLAG</LG> <Y>)</Y>; <G>// no to yes, yes to no</G>
<G>// flag is_ready = no;</G>
<G>// flip( is_ready );</G>
<G>// is_ready is now yes</G>

<M>pick</M><Y>(</Y> <LG>FLAG</LG>, <LG>A</LG>, <LG>B</LG> <Y>)</Y> <G>// Ternary operator</G>
<G>// If FLAG is yes, pick A, else pick B</G>
</pre>

### Type Prefixes
<pre>
<Y>temp</Y> <LG>TYPE VAR</LG> <G>// VAR is temporary</G>
<G>// Cannot get ref_of a temp variable!</G>

<Y>perm</Y> <LG>TYPE VAR</LG> <G>// VAR is permanent</G>
<G>// Made once, always exists in the scope</G>

<Y>global</Y> <LG>TYPE VAR</LG> <G>// VAR is explicitly global</G>
<G>// Used as a label outside functions/scopes</G>
<G>// Globals are discouraged,</G>
<G>//  this is to make them explicit</G>
</pre>

-------
## Control Flow

### Loops
<pre>
<M>loop</M>
<C>{</C>
	<G>// Infinite loop</G>
<C>}</C>

<M>while</M><Y>(</Y> x <Y>)</Y>
<C>{</C>
	<G>// Run this code while x is yes</G>
<C>}</C>

<M>do</M>
<C>{</C>
	<G>// Run this code</G>
	<G>// While x is yes</G>
<C>}</C>
<M>while</M><Y>(</Y> x <Y>)</Y>;

<M>do</M>
<C>{</C>
	<G>// Run this code</G>
	<G>// Until x is yes</G>
<C>}</C>
<M>until</M><Y>(</Y> x <Y>)</Y>;

<M>next</M> <G>// Jump up to next iteration</G>
<M>skip</M> <G>// Skip the rest of the scope</G>

<LG>POINT</LG>: <G>// Set jump point</G>
<M>jump</M> <LG>POINT</LG>; <G>// Jump to POINT
</pre>

### Iteration Helpers

<pre>
<G>// Range functions include from and to</G>
<M>range</M><Y>(</Y> VAR, FROM, TO <Y>)</Y>
<C>{</C>
	<G>// Iterates VAR in a FROM-TO range</G>
	<G>// Progresses 1 at a time</G>
	<G>// ( i, 2, 7 ) makes i go from 2 to 7</G>
<C>}</C>

<M>range_step</M><Y>(</Y> VAR, FROM, TO, STEP <Y>)</Y>
<C>{</C>
	<G>// Iterates VAR in a FROM-TO range,</G>
	<G>//  but progresses with STEP</G>
<C>}</C>

<G>// Iter functions always start from 0</G>
<M>iter</M><Y>(</Y> VAR, SIZE <Y>)</Y>
<C>{</C>
	<G>// Iterates VAR from 0 to SIZE-1</G>
	<G>// Progresses 1 at a time</G>
<C>}</C>

<M>iter_step</M><Y>(</Y> VAR, SIZE, STEP <Y>)</Y>
<C>{</C>
	<G>// Iterates VAR from 0 to SIZE-1,</G>
	<G>//  but progresses with STEP</G>
<C>}</C>

<M>iter_grid</M><Y>(</Y> X, Y, WIDTH, HEIGHT <Y>)</Y>
<C>{</C>
	<G>// Iterates X from 0 to WIDTH-1,</G>
	<G>//  and Y from 0 to HEIGHT-1</G>
	<G>// Left-to-right, top-to-bottom</G>
	<G>// For things like pixel images</G>
<C>}</C>

<M>repeat</M><Y>(</Y> n <Y>)</Y>
<C>{</C>
	<G>// Repeats this scope n-times</G>
<C>}</C>

<M>once</M>
<C>{</C>
	<G>// This scope runs only once</G>
	<G>// Used for testing or initializing</G>
<C>}</C>
</pre>

### with/when Statements
<pre>
<G>// Jump to a when() depending on what it is</G>
<M>with</M><Y>(</Y> val <Y>)</Y>
<C>{</C>
	<M>when</M><Y>(</Y> <C>1</C>, <C>2</C>, <C>3</C> <Y>)</Y>
	<C>{</C>
		<G>// Code only if val is 1, 2, or 3</G>
		<M>skip</M>; <G>// Skip the rest</G>
	<C>}</C>
	
	<M>when</M><Y>(</Y> <C>4</C> <Y>)</Y>
	<C>{</C>
		<G>// Code specifically for if val is 4</G>
	<C>}</C> <G>// There's no skip, so it continues:</G>
	
	<M>other</M>
	<C>{</C>
		<G>// This code is ran if val</G>
		<G>//  isn't 1, 2, or 3.
		<G>// But will if it's 4 or anything else</G>
	<C>}</C> <G>// Skip isn't required at the end
<C>}</C>
<G>// All skips takes us out to here</G>
</pre>

### Conditional Helpers
<pre>
<M>if_nothing</M><Y>( </Y>REF<Y> )</Y>
<C>{</C>
    <G>// If REF is nothing</G>
<C>}</C>

<M>if_something</M><Y>( </Y>REF<Y> )</Y>
<C>{</C>
    <G>// If REF is not nothing</G>
<C>}</C>

<M>if_any</M><Y>(</Y> a, b, c... <Y>)</Y>
<C>{</C>
    <G>// If any arguments are yes</G>
<C>}</C>

<M>if_all</M><Y>(</Y> a, b, c... <Y>)</Y>
<C>{</C>
    <G>// If all arguments are yes</G>
<C>}</C>

<M>if_none</M><Y>(</Y> a, b, c... <Y>)</Y>
<C>{</C>
    <G>// If none of the arguments are yes</G>
<C>}</C>

<M>skip_if</M><Y>(</Y> x <Y>)</Y>; <G>// Skip if x is yes</G>
<M>next_if</M><Y>(</Y> x <Y>)</Y>; <G>// Jump to next if x is yes</G>
<M>out_if</M><Y>(</Y> x <Y>)</Y> v; <G>// Output v if x is yes</G>
<G>// Can be empty if function doesn't output:</G>
<G>// out_if( x );</G>
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

### Embed Prefix

```
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

-------
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

-------
## Example Programs

### Hello World
<pre>
<Y>#include</Y> <C>"H.h"</C>

<M>start</M>
<C>{</C>
	<M>print</M><Y>(</Y> <C>"Hello, World!\n"</C> <Y>)</Y>;
	<M>out executable_success</M>;
<C>}</C>
</pre>

### File Processing
<pre>
<Y>#include</Y> <C>"H.h"</C>

<M>start</M>
<C>{</C>
	<Y>file</Y> f <Y>=</Y> <M>open_file</M><Y>(</Y> input_bytes<C>[</C> <C>1</C> <C>]</C> <Y>)</Y>;
	<M>if_nothing</M><Y>(</Y> f.handle <Y>)</Y>
	<C>{</C>
		<M>print</M><Y>(</Y> <C>"Failed to open file\n"</C> <Y>)</Y>;
		<M>out</M> <M>executable_failure</M>;
	<C>}</C>
	
	<Y>byte</Y> input<C>[</C> <M>KB</M><Y>(</Y> <C>100</C> <Y>)</Y> <C>]</C>;
	<M>temp</M> <Y>byte</Y> <Y>ref</Y> input_ref <Y>=</Y> input;
	<M>file_load</M><Y>(</Y> f, input <Y>)</Y>;
	
	<Y>byte</Y> output<C>[</C> <M>KB</M><Y>(</Y> <C>100</C> <Y>)</Y> <C>]</C>;
	<M>temp</M> <Y>byte</Y> <Y>ref</Y> output_ref <Y>=</Y> output;
	
	<Y>check_input</Y>:
	<C>{</C>
		<M>temp</M> <Y>byte</Y> val <Y>=</Y> <M>val_of</M><Y>(</Y> input_ref <Y>)</Y>;
		<M>with</M><Y>(</Y> val <Y>)</Y>
		<C>{</C>
			<M>when</M><Y>(</Y> <C>'\0'</C> <Y>)</Y> <M>jump</M> <Y>input_eof</Y>;

			<M>when</M><Y>(</Y> <C>' '</C>, <C>'\t'</C>, <C>'\n'</C>, <C>'\r'</C> <Y>)</Y> <M>skip</M>;

			<M>when</M><Y>(</Y> <C>'E'</C>, <C>'e'</C> <Y>)</Y>
			<C>{</C>
				<M>val_of</M><Y>(</Y> output_ref <Y>)</Y> <Y>=</Y> <C>'3'</C>;
				output_ref <Y>=</Y> output_ref <Y>+</Y> <C>1</C>;
				<M>skip</M>;
			<C>}</C>

			<M>other</M>
			<C>{</C>
				<M>val_of</M><Y>(</Y> output_ref <Y>)</Y> <Y>=</Y> val;
				output_ref <Y>=</Y> output_ref <Y>+</Y> <C>1</C>;
				<M>skip</M>;
			<C>}</C>
		<C>}</C>
		input_ref <Y>=</Y> input_ref <Y>+</Y> <C>1</C>;
		<M>jump</M> <Y>check_input</Y>;
	<C>}</C>

	<Y>input_eof</Y>:
	<M>file_close</M><Y>(</Y> f <Y>)</Y>;
	<M>print</M><Y>(</Y> output <Y>)</Y>;
	<M>print_newline</M><Y>()</Y>;
	<M>out</M> <M>executable_success</M>;
<C>}</C>
</pre>

## License

H is released under CC0 (Creative Commons Zero) - effectively public domain. FOSS forever.
 
