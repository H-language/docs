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
- Unified file and folder I/O
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
## Bytes, References, and Types
<H>H</H> is "only bytes and references".

## a <H>byte</H> is <i>8 bits</i>
## a <H>ref</H> is <i>8 bytes</i>

### Bytes
If you're dealing with bytes, use:
<pre>
<Y>byte</Y>
<G>// byte x = '7';</G>
<G>// The '7' character is 55</G>
<G>// Which is 00110111 in bits</G>
</pre>

### Refs
<pre>
<LG>TYPE</LG> <Y>ref</Y> <LG>NAME</LG> <G>// A reference to a TYPE</G>
</pre>
| H Syntax | Definition |
|----------|-------------|
| <code><M>ref_of</M><Y>(</Y> <LG>VAR</LG> <Y>)</Y></code> | Reference-of VAR |
| <code><M>val_of</M><Y>(</Y> <LG>REF</LG> <Y>)</Y></code> | Value-of REF |
| <code><M>to</M><Y>(</Y> <LG>TYPE</LG>, <LG>VAL</LG> <Y>)</Y></code> | Changes VAL to TYPE |
| <code><M>cast</M><Y>(</Y> <LG>TYPE</LG>, <LG>VAR</LG> <Y>)</Y></code> | Reinterpret VAR to TYPE |

If the reference type is unknown:
<pre>
<Y>anon</Y>
<G>// some_type ref y_ref = ref_of( y );</G>
<G>// anon ref x = to( anon ref, y_ref );</G>
</pre>

If the reference itself is unknown:
<pre>
<C>nothing</C>
<G>// byte ref x = nothing;</G>
</pre>

### Number Types
- <H>N</H>atural <LG>(cannot be less than zero)</LG>
- <H>I</H>nteger <LG>(can be negative)</LG>
- <H>R</H>ational <LG>(has a fractional part)</LG>

<pre>
<LG>[</LG><Y>n</Y><LG>/</LG><Y>i</Y><LG>/</LG><Y>r</Y><LG>][</LG><Y>size</Y><LG>]</LG>
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
<Y>perm</Y> <LG>TYPE VAR</LG> <G>// VAR is permanent</G>
<G>// Made once, always exists in the scope</G>

<Y>temp</Y> <LG>TYPE VAR</LG> <G>// VAR is temporary</G>
<G>// Cannot get ref_of a temp variable!</G>

<Y>const</Y> <LG>TYPE NAME</LG> <G>// NAME is constant</G>
<G>// Cannot be changed!

<Y>global</Y> <LG>TYPE VAR</LG> <G>// VAR is explicitly global</G>
<G>// Used as a label outside functions/scopes</G>
<G>// Globals are discouraged,</G>
<G>//  this is to make them explicit</G>
</pre>
They can be combined:
<pre>
<Y>perm</Y><LG>/</LG><Y>temp const</Y> <LG>TYPE NAME</LG>
<G>// A permanent-or-temporary constant TYPE</G>
<G>// perm/temp always go before const</G>
</pre>

### Logic Operations
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
## Functions

### Function Declaration
If the function doesn't output anything:
<pre>
<M>fn</M> <LG>NAME</LG><Y>(</Y> <LG>TYPE A</LG>, <LG>TYPE B</LG>, <LG>TYPE C...</LG> <Y>)</Y>
<C>{</C>
	<G>// function code</G>
	<M>out</M>; <G>// exits the function</G>
<C>}</C>
</pre>
If the function does output something:
<pre>
<LG>TYPE</LG> <LG>NAME</LG><Y>(</Y> <LG>TYPE A</LG>, <LG>TYPE B</LG>, <LG>TYPE C...</LG> <Y>)</Y>
<C>{</C>
	<G>// function code</G>
	<M>out</M> <LG>VAL</LG>; <G>// outputs a value of TYPE</G>
<C>}</C>
</pre>

### Embed Function Prefix
<pre>
<M>embed</M> <LG>TYPE</LG> <LG>NAME</LG><Y>(</Y> <LG>...</LG> <Y>)</Y>
<G>// This will force the compiler to embed</G>
<G>//  the function code in where it's called</G>
</pre>

-------
## Control Flow

### Loops
<pre>
<M>loop</M>
<C>{</C>
	<G>// Infinite loop</G>
	<G>// Requires a skip or jump to escape!</G>
<C>}</C>
<G>// loop{}; will freeze the executable</G>

<M>while</M><Y>(</Y> <LG>FLAG</LG> <Y>)</Y>
<C>{</C>
	<G>// Run this code while FLAG is yes</G>
<C>}</C>

<M>do</M>
<C>{</C>
	<G>// Run this code</G>
	<G>// While FLAG is yes</G>
<C>}</C>
<M>while</M><Y>(</Y> <LG>FLAG</LG> <Y>)</Y>;

<M>do</M>
<C>{</C>
	<G>// Run this code</G>
	<G>// Until FLAG is yes</G>
<C>}</C>
<M>until</M><Y>(</Y> <LG>FLAG</LG> <Y>)</Y>;

<M>skip</M> <G>// Skip the rest of the scope</G>

<LG>POINT</LG>: <G>// Set jump point</G>
<M>jump</M> <LG>POINT</LG>; <G>// Jump to POINT</G>

<M>next</M> <G>// Jump up to next iteration</G>
<G>// while( x < 10 )</G>
<G>// {</G>
<G>// 	x = x + 1;</G>
<G>// 	if( x < 5 )</G>
<G>// 	{</G>
<G>// 		next; // jumps back up to while</G>
<G>// 	}</G>
<G>// 	...</G>
<G>// }</G>

<M>skip_if</M><Y>(</Y> <LG>F</LG> <Y>)</Y>; <G>// Skip if F is yes</G>
<M>jump_if</M><Y>(</Y> <LG>F</LG> <Y>)</Y> <LG>P</LG>; <G>// Jump to P if F is yes</G>
<M>next_if</M><Y>(</Y> <LG>F</LG> <Y>)</Y>; <G>// Jump to next if F is yes</G>
<M>out_if</M><Y>(</Y> <LG>F</LG> <Y>)</Y> v; <G>// Output v if F is yes</G>
<G>// Can be empty if function doesn't output:</G>
<G>// out_if( is_done isnt no );</G>
</pre>

### Iteration Helpers

<pre>
<G>// Range functions include from and to</G>
<M>range</M><Y>(</Y> <LG>VAR</LG>, <LG>FROM</LG>, <LG>TO</LG> <Y>)</Y>
<C>{</C>
	<G>// Iterates VAR in a FROM-TO range</G>
	<G>// Progresses 1 at a time</G>
	<G>// ( i, 2, 7 ) makes i go from 2 to 7</G>
<C>}</C>

<M>range_step</M><Y>(</Y> <LG>VAR</LG>, <LG>FROM</LG>, <LG>TO</LG>, <LG>STEP</LG> <Y>)</Y>
<C>{</C>
	<G>// Iterates VAR in a FROM-TO range,</G>
	<G>//  but progresses with STEP</G>
<C>}</C>

<G>// Iter functions always start from 0</G>
<M>iter</M><Y>(</Y> <LG>VAR</LG>, <LG>SIZE</LG> <Y>)</Y>
<C>{</C>
	<G>// Iterates VAR from 0 to SIZE-1</G>
	<G>// Progresses 1 at a time</G>
<C>}</C>

<M>iter_step</M><Y>(</Y> <LG>VAR</LG>, <LG>SIZE</LG>, <LG>STEP</LG> <Y>)</Y>
<C>{</C>
	<G>// Iterates VAR from 0 to SIZE-1,</G>
	<G>//  but progresses with STEP</G>
<C>}</C>

<M>iter_grid</M><Y>(</Y> <LG>X</LG>, <LG>Y</LG>, <LG>WIDTH</LG>, <LG>HEIGHT</LG> <Y>)</Y>
<C>{</C>
	<G>// Iterates X from 0 to WIDTH-1,</G>
	<G>//  and Y from 0 to HEIGHT-1</G>
	<G>// Left-to-right, top-to-bottom</G>
	<G>// For things like pixel images</G>
<C>}</C>

<M>repeat</M><Y>(</Y> <LG>N</LG> <Y>)</Y>
<C>{</C>
	<G>// Repeats this scope N-times</G>
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
<M>with</M><Y>(</Y> <LG>VAL</LG> <Y>)</Y>
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
<M>if_nothing</M><Y>(</Y> <LG>REF</LG> <Y>)</Y>
<C>{</C>
	<G>// If REF is nothing</G>
<C>}</C>

<M>if_something</M><Y>(</Y> <LG>REF</LG> <Y>)</Y>
<C>{</C>
	<G>// If REF is not nothing</G>
<C>}</C>

<M>if_any</M><Y>(</Y> <LG>A</LG>, <LG>B</LG>, <LG>C...</LG> <Y>)</Y>
<C>{</C>
	<G>// If any arguments are yes</G>
<C>}</C>

<M>if_all</M><Y>(</Y> <LG>A</LG>, <LG>B</LG>, <LG>C...</LG> <Y>)</Y>
<C>{</C>
	<G>// If all arguments are yes</G>
<C>}</C>

<M>if_none</M><Y>(</Y> <LG>A</LG>, <LG>B</LG>, <LG>C...</LG> <Y>)</Y>
<C>{</C>
	<G>// If none of the arguments are yes</G>
<C>}</C>
</pre>

-------
## Memory Operations

### Byte Reference Operations
<pre>
<M>bytes_copy</M><Y>(</Y> <LG>FROM</LG>, <LG>SIZE</LG>, <LG>TO</LG> <Y>)</Y>
<G>// Copy SIZE bytes from FROM to TO</G>

<M>bytes_copy_internal</M><Y>(</Y> <LG>FROM</LG>, <LG>SIZE</LG>, <LG>TO</LG> <Y>)</Y>
<G>// Copy SIZE bytes, handles overlapping</G>

<M>bytes_fill</M><Y>(</Y> <LG>PTR</LG>, <LG>VALUE</LG>, <LG>SIZE</LG> <Y>)</Y>
<G>// Fill SIZE bytes with VALUE</G>

<M>bytes_clear</M><Y>(</Y> <LG>PTR</LG>, <LG>SIZE</LG> <Y>)</Y>
<G>// Clear SIZE bytes to zero</G>

<M>bytes_compare</M><Y>(</Y> <LG>A</LG>, <LG>B</LG>, <LG>SIZE</LG> <Y>)</Y>
<G>// Compare SIZE bytes</G>
<G>// Outputs 0 if equal,</G>
<G>//  negative if A < B, positive if A > B</G>
</pre>

### Null-Terminated Operations
<pre>
<M>bytes_measure</M><Y>(</Y> <LG>STR</LG> <Y>)</Y>
<G>// Count bytes until '\0'</G>

<M>bytes_paste</M><Y>(</Y> <LG>FROM</LG>, <LG>TO</LG> <Y>)</Y>
<G>// Copy bytes until '\0'</G>

<M>bytes_end</M><Y>(</Y> <LG>REF</LG> <Y>)</Y>
<G>// Set REF byte to '\0'</G>
<G>// Doesn't measure REF!</G>
</pre>

### Advanced Copy Operations
<pre>
<M>bytes_copy_move</M><Y>(</Y> <LG>FROM</LG>, <LG>SIZE</LG>, <LG>TO</LG> <Y>)</Y>
<G>// Copy SIZE bytes then</G>
<G>//  moves the TO ref by SIZE</G>
<M>bytes_paste_move</M><Y>(</Y> <LG>FROM</LG>, <LG>TO</LG> <Y>)</Y>
<G>// Pastes null-terminated FROM to TO</G>
<G>//  moves the TO ref by FROM size</G>
<M>bytes_set_move</M><Y>(</Y> <LG>BYTE</LG>, <LG>TO</LG> <Y>)</Y>
<G>// Set single byte and move the TO ref by 1</G>
</pre>

-------
## Math Operations

<pre>
<M>MIN</M><Y>(</Y> <LG>A</LG>, <LG>B</LG> <Y>)</Y> <G>// Smaller of A or B</G>
<M>MAX</M><Y>(</Y> <LG>A</LG>, <LG>B</LG> <Y>)</Y> <G>// Larger of A or B</G>
<M>MIN3</M><Y>(</Y> <LG>A</LG>, <LG>B</LG>, <LG>C</LG> <Y>)</Y> <G>// Smallest of three</G>
<M>MIN4</M><Y>(</Y> <LG>A</LG>, <LG>B</LG>, <LG>C</LG>, <LG>D</LG> <Y>)</Y> <G>// Smallest of four</G>
<M>MAX3</M><Y>(</Y> <LG>A</LG>, <LG>B</LG>, <LG>C</LG> <Y>)</Y> <G>// Largest of three</G>
<M>MAX4</M><Y>(</Y> <LG>A</LG>, <LG>B</LG>, <LG>C</LG>, <LG>D</LG> <Y>)</Y> <G>// Largest of four</G>
<M>MEAN</M><Y>(</Y> <LG>A</LG>, <LG>B</LG> <Y>)</Y> <G>// Average of A and B</G>
<M>CLAMP</M><Y>(</Y> <LG>VAL</LG>, <LG>MIN</LG>, <LG>MAX</LG> <Y>)</Y> <G>// Keep VAL between MIN and MAX</G>
<M>ABS</M><Y>(</Y> <LG>VAL</LG> <Y>)</Y> <G>// Remove negative sign</G>
<M>SIGN</M><Y>(</Y> <LG>VAL</LG> <Y>)</Y> <G>// Returns 1 or -1</G>
<M>SQR</M><Y>(</Y> <LG>VAL</LG> <Y>)</Y> <G>// VAL times VAL</G>
<M>CUBE</M><Y>(</Y> <LG>VAL</LG> <Y>)</Y> <G>// VAL times VAL times VAL</G>
<M>SNAP</M><Y>(</Y> <LG>VAL</LG>, <LG>MULTIPLE</LG> <Y>)</Y> <G>// Round VAL to nearest MULTIPLE</G>
</pre>

-------
## Structures and Objects

### Basic Struct

<pre>
<M>struct</M><Y>(</Y><LG>Point</LG><Y>)</Y>
<C>{</C>
    <Y>r4</Y> <LG>x</LG>, <LG>y</LG>;
<C>}</C>;

<G>// Usage:</G>
<LG>Point</LG> <LG>p</LG> = <M>make</M><Y>(</Y><LG>Point</LG>, <C>10.0</C>, <C>20.0</C><Y>)</Y>;
</pre>

### Objects with Methods

<pre>
<M>object</M><Y>(</Y><LG>Entity</LG><Y>)</Y>
<C>{</C>
    <Y>i4</Y> <LG>id</LG>;
    <Y>byte</Y> <LG>name</LG><Y>[</Y><C>32</C><Y>]</Y>;
<C>}</C>;

<M>object_fn</M><Y>(</Y><LG>Entity</LG>, <LG>update</LG><Y>)</Y>
<C>{</C>
    <G>// 'this' is auto-available as Entity ref</G>
    <LG>this</LG><Y>-></Y><LG>id</LG><Y>++</Y>;
<C>}</C>

<G>// Usage:</G>
<LG>Entity</LG> <LG>e</LG> = <C>{0}</C>;
<M>call</M><Y>(</Y><M>ref_of</M><Y>(</Y><LG>e</LG><Y>)</Y>, <LG>update</LG><Y>)</Y>; <G>// Calls if not nothing</G>
</pre>

### Fusion and Group

<pre>
<M>fusion</M><Y>(</Y><LG>Value</LG><Y>)</Y> <G>// Can hold different types</G>
<C>{</C>
    <Y>i4</Y> <LG>integer</LG>;
    <Y>r4</Y> <LG>rational</LG>;
<C>}</C>;

<M>group</M><Y>(</Y><LG>Color</LG>, <Y>n1</Y><Y>)</Y> <G>// Named values with type</G>
<C>{</C>
    <LG>red</LG>,
    <LG>green</LG>,
    <LG>blue</LG>
<C>}</C>;
</pre>

-------
## File I/O

### File Operations

<pre>
<G>// Open files</G>
<Y>file</Y> <LG>f</LG> = <M>open_file_loading</M><Y>(</Y><C>"path.txt"</C><Y>)</Y>;
<Y>file</Y> <LG>f</LG> = <M>open_file_saving</M><Y>(</Y><C>"output.txt"</C><Y>)</Y>;

<G>// Load/Save</G>
<Y>byte</Y> <LG>buffer</LG><Y>[</Y><C>1024</C><Y>]</Y>;
<M>file_load</M><Y>(</Y><M>ref_of</M><Y>(</Y><LG>f</LG><Y>)</Y>, <LG>buffer</LG><Y>)</Y>;
<M>file_save</M><Y>(</Y><M>ref_of</M><Y>(</Y><LG>f</LG><Y>)</Y>, <LG>DATA</LG>, <LG>SIZE</LG><Y>)</Y>;

<G>// Memory mapped files</G>
<Y>file</Y> <LG>f</LG> = <M>map_file</M><Y>(</Y><C>"large.dat"</C>, <LG>PATH_SIZE</LG><Y>)</Y>;
<G>// Access via f.mapped_bytes</G>
<M>file_unmap</M><Y>(</Y><M>ref_of</M><Y>(</Y><LG>f</LG><Y>)</Y><Y>)</Y>;

<G>// Cleanup</G>
<M>file_close</M><Y>(</Y><M>ref_of</M><Y>(</Y><LG>f</LG><Y>)</Y><Y>)</Y>;
<M>file_delete</M><Y>(</Y><C>"path.txt"</C><Y>)</Y>;
</pre>

### File Utilities

<pre>
<Y>flag</Y> <M>file_exists</M><Y>(</Y><LG>PATH</LG><Y>)</Y>
<Y>flag</Y> <M>folder_exists</M><Y>(</Y><LG>PATH</LG><Y>)</Y>
<Y>n8</Y> <M>get_file_size</M><Y>(</Y><LG>PATH</LG><Y>)</Y>
<M>create_folder</M><Y>(</Y><LG>PATH</LG><Y>)</Y>
</pre>

### Directory Operations

<pre>
<Y>byte</Y> <LG>entries</LG><Y>[</Y><C>100</C><Y>][</Y><LG>PATH_MAX_SIZE</LG><Y>]</Y>;
<Y>n2</Y> <LG>count</LG>;

<G>// Get all entries</G>
<LG>count</LG> = <M>get_entries_in_folder</M><Y>(</Y><LG>DIR</LG>, <LG>entries</LG>, <C>100</C>, <LG>entry_any</LG><Y>)</Y>;

<G>// Get only files</G>
<LG>count</LG> = <M>get_files</M><Y>(</Y><LG>DIR</LG>, <LG>entries</LG>, <C>100</C><Y>)</Y>;

<G>// Get only folders</G>
<LG>count</LG> = <M>get_folders</M><Y>(</Y><LG>DIR</LG>, <LG>entries</LG>, <C>100</C><Y>)</Y>;
</pre>

### Path Operations

<pre>
<M>path</M><Y>(</Y><C>"folder"</C>, <C>"subfolder"</C>, <C>"file.txt"</C><Y>)</Y>
<G>// Makes "folder/subfolder/file.txt"</G>
<M>path_up_folder</M><Y>(</Y><LG>PATH_BUFFER</LG><Y>)</Y> <G>// Remove last part</G>
<Y>byte ref</Y> <LG>exe_path</LG> = <M>get_exe_path</M><Y>()</Y> <G>// Program location</G>
</pre>

-------
## Terminal Operations

### Basic Output

<pre>
<M>print</M><Y>(</Y><C>"Hello, World!"</C><Y>)</Y> <G>// Output text</G>
<M>print_size</M><Y>(</Y><LG>BUFFER</LG>, <C>100</C><Y>)</Y> <G>// Output exact bytes</G>
<M>print_newline</M><Y>()</Y> <G>// Start new line</G>
</pre>

### Formatted Output

<pre>
<M>print_format</M><Y>(</Y><C>"Value: &lt;c:g&gt;%d&lt;/c:g&gt;\n"</C>, <C>42</C><Y>)</Y>;

<G>// Format tags:</G>
<G>// &lt;c:r&gt; = red, &lt;c:g&gt; = green, &lt;c:b&gt; = blue</G>
<G>// &lt;c:y&gt; = yellow, &lt;c:m&gt; = magenta, &lt;c:c&gt; = cyan</G>
<G>// &lt;b&gt; = bold, &lt;u&gt; = underline</G>
</pre>

### Format Bytes Function

<pre>
<G>// Format to buffer with size tracking</G>
<Y>byte</Y> <LG>buffer</LG><Y>[</Y><C>256</C><Y>]</Y>;
<Y>n4</Y> <LG>size</LG> = <M>format_bytes</M><Y>(</Y><LG>buffer</LG>, <C>"Name: <c:b><></c:b>"</C>, <C>"John"</C><Y>)</Y>;

<G>// Format and advance pointer</G>
<Y>byte ref</Y> <LG>ptr</LG> = <LG>buffer</LG>;
<M>format_bytes_move</M><Y>(</Y><LG>ptr</LG>, <C>"Value: <c:g>%d</c:g>\n"</C>, <C>42</C><Y>)</Y>;
</pre>

### Terminal Colors

<pre>
<M>terminal_set_color</M><Y>(</Y><LG>red</LG><Y>)</Y>
<M>terminal_set_bg_color</M><Y>(</Y><LG>blue</LG><Y>)</Y>
<M>terminal_set_bold</M><Y>()</Y>
<M>terminal_reset_format</M><Y>()</Y>
<M>terminal_clear</M><Y>()</Y>
</pre>

### Terminal Input

<pre>
<Y>byte ref</Y> <LG>input</LG> = <M>get_terminal_input</M><Y>()</Y>;
<G>// Returns user typed text</G>
</pre>

-------
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

-------
## Platform Abstraction

<H>H</H> automatically detects and adapts to the platform:

<pre>
<LG>OS_LINUX</LG> <G>// 1 if Linux, 0 otherwise</G>
<LG>OS_WINDOWS</LG> <G>// 1 if Windows, 0 otherwise</G>
<LG>OS_MACOS</LG> <G>// 1 if macOS, 0 otherwise</G>
<LG>OS_NAME</LG> <G>// String: "Linux", "Windows", "macOS"</G>
<LG>SEPARATOR</LG> <G>// "/" on Unix, "\\" on Windows</G>
</pre>

-------
## Memory Size Constants

<pre>
<M>KB</M><Y>(</Y> <LG>N</LG> <Y>)</Y> <G>// N * 1,000</G>
<M>MB</M><Y>(</Y> <LG>N</LG> <Y>)</Y> <G>// N * 1,000,000</G>
<M>GB</M><Y>(</Y> <LG>N</LG> <Y>)</Y> <G>// N * 1,000,000,000</G>
<M>KiB</M><Y>(</Y> <LG>N</LG> <Y>)</Y> <G>// N * 1,024</G>
<M>MiB</M><Y>(</Y> <LG>N</LG> <Y>)</Y> <G>// N * 1,048,576</G>
<M>GiB</M><Y>(</Y> <LG>N</LG> <Y>)</Y> <G>// N * 1,073,741,824</G>
</pre>

-------
## Utility Macros

### Default Arguments
<pre>
<M>DEFAULT</M><Y>(</Y> <LG>VAL</LG>, <LG>ARG</LG> <Y>)</Y>
<G>// Becomes ARG if provided, otherwise VAL</G>

<M>DEFAULTS</M><Y>(</Y> <Y>(</Y> <LG>TUPLE</LG> <Y>)</Y>, <LG>ARGS...</LG> <Y>)</Y>
<G>// Used like:</G>
<C>#define</C> <M>foo</M><Y>(</Y> <LG>NAME</LG>, <LG>ARGS...</LG> <Y>)\</Y>
	<LG>NAME</LG><Y>(</Y> <M>DEFAULTS</M><Y>(</Y> <Y>(</Y> <C>1</C>, <C>2</C>, <C>3</C> <Y>)</Y>, <LG>ARGS</LG> <Y>)</Y> <Y>)</Y>
<G>// foo( test ) -> test( 1, 2, 3 )</G>
<G>// foo( test, 7 ) -> test( 7, 2, 3 )</G>
<G>// foo( test, 7, 8 ) -> test( 7, 8, 3 )</G>
<G>// foo( test, 7, 8, 9 ) -> test( 7, 8, 9 )</G>
<G>// foo( test, 7,, 9 ) -> test( 7,, 9 )</G>
</pre>

### Compile-Time Helpers
<pre>
<M>COUNT_ARGS</M><Y>(</Y> <LG>A</LG>, <LG>B</LG>, <LG>C</LG> <Y>)</Y> <G>// Returns 3</G>
<M>STRINGIFY</M><Y>(</Y> <LG>hello</LG> <Y>)</Y> <G>// Converts to "hello"</G>
<M>CHAIN</M><Y>(</Y> <LG>L</LG>, <LG>R</LG>, <LG>MID</LG>, <LG>ARGS...</LG> <Y>)</Y> <G>// L arg1 R MID L arg2 R</G>
</pre>

### Byte Array Declaration
<pre>
<M>declare_bytes</M><Y>(</Y> <LG>buffer</LG>, <M>KB</M><Y>(</Y> <C>1</C> <Y>)</Y> <Y>)</Y>;
<G>// Creates empty buffer[1000]</G>
<G>// And buffer_ref that's at [0]</G>

<M>declare_bytes</M><Y>(</Y> <LG>msg</LG>, <C>32</C>, <C>"Hello"</C> <Y>)</Y>;
<G>// Creates msg[32] = "Hello"</G>
<G>// And msg_ref that's after the 'o'</G>

<LG>msg</LG><Y>[</Y><C>0</C><Y>]</Y> = <C>'7'</C>; <G>// msg is now "7ello"</G>
<LG>msg_ref</LG><Y>[</Y><C>0</C><Y>]</Y> = <C>'E'</C>; <G>// msg is now "7elloE"</G>
</pre>

### Character Classification

<pre>
<M>is_letter</M><Y>(</Y> <LG>BYTE</LG> <Y>)</Y> <G>// Check if alphabetic</G>
<M>is_number</M><Y>(</Y> <LG>BYTE</LG> <Y>)</Y> <G>// Check if numeric</G>
</pre>

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

-------
## License

H is released under CC0 (Creative Commons Zero) - effectively public domain. FOSS forever.
 
-------
