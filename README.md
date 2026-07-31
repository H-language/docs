# <H>H</H>ydrogen <H>Lang</H>uage Documentation

#### <i><G>created by </G><a href="https://x.com/ENDESGA" class="EDG">ENDESGA</a><G> - started in 2020 - made in NZ - CC0 - FOSS forever</G></i>

-------
## <G>a quick overview</G>
<H>H</H> is a single-file syntactic layer that reshapes C into a more readable and understandable language.
<H>H</H>ydrogen <H>Lang</H>uage provides:
- Intuitive and explicit keywords
- Functional programming patterns
- Cross-platform abstractions
- Zero overhead bloat
- Single header file implementation
- Unified file and folder I/O
- Modern functions and data-structures

### the questions that began H, and a personal statement:
Why go to all the effort of making a programming language when it's not even readable for the average person? What's the point of gatekeeping "talking to computers" behind walls of unintuitive symbols? It's hard enough learning the logic, rules, and workflows to program effectively. Why make it harder with symbols and cryptic words that require remembering rather than simply reading?

Programmers forget how difficult it was to initially understand how to code, and brush it off as "you'll learn and struggle too". But why not just bring the barrier of entry down by making a language readable from the start?

My answer to this is an abstraction layer I've been crafting and honing for many years.
This is designed for younger me, who struggled to read and understand code for a very long time. The over-use of symbols in programming languages has frustrated me since the moment I started learning. Only after many years of intense study, practice, and fixation have I finally got enough knowledge to create a solution.

Create what you wish existed.

<LG>— End</LG>

-------
## Contents

<toc></toc>

-------
## Installation
Simply include H.h in your C project:
<pre>
<Y>#include</Y> <C>"H.h"</C>
</pre>
Then compile with <H>GCC</H>, or <H>TCC</H>.

Everything <H>H</H> gives you is in that one file. There's nothing to link, nothing to build, and nothing to configure.

-------
## Language Basics

### How Your Computer "Reads"
Your code gets compiled down from human-readable text to <b>Assembly</b>.
Assembly is a computer-readable compact list of operation codes that the CPU itself can act upon.
The CPU executes these operations one at a time, and the path that it takes directly correlates to the human-readable code you type.

There's no magical interpretation happening like in scripting languages, <H>H</H> is directly turned into something your computer can process immediately.

### Reading Order
The computer reads from <b>top to bottom</b>. Each statement ends with a <G>"</G><C>;</C><G>"</G>, similar to a punctuation-mark/period at the end of English sentences:
<pre>
<LG>first statement</LG><C>;</C>  <G>// CPU does this first</G>
<LG>second statement</LG><C>;</C> <G>// Then this</G>
<LG>third statement</LG><C>;</C>  <G>// Then this</G>
<LG>...</LG>
</pre>

Nothing can be used before it's been defined above it. A file reads like a book: top to bottom, no page-flipping.

### Code Scopes
A scope <G>"</G><C>{</C><G>...</G><C>}</C><G>"</G> contains statements, and defines where things exist.
Variables created inside a scope disappear when the scope ends:
<pre>
<C>{</C> <G>// Scope starts here</G>
	<LG>x exists</LG><C>;</C> <G>// x is created here</G>
	<LG>y exists</LG><C>;</C> <G>// y is created here</G>
<C>}</C> <G>// Scope ends: x and y are destroyed</G>
<G>// x and y no longer exist</G>
</pre>

### Memory and Storage
Variables reserve memory to store values.
The <Y>=</Y> operator copies values into variables:
<pre>
<LG>year</LG> <Y>=</Y> <C>2025;</C>		 <G>// Copy 2025 to 'year'</G>
<LG>letter</LG> <Y>=</Y> <C>'H';</C>		 <G>// Copy 'H' to 'letter'</G>
<LG>name</LG> <Y>=</Y> <C>"Britta";</C> <G>// Copy "Britta" to 'name'</G>
</pre>

### Comments Are Ignored
Use <C>//</C> for notes that won't become CPU instructions.
These are completely ignored by the compiler:
<pre>
<LG>some statement</LG><C>;</C> <G>// This comment is skipped</G>
<G>/* Multi-line comments
are also skipped */</G>
<LG>another statement</LG><C>;</C>
<LG>...</LG>
</pre>

-------
## Bytes and References
<H>H</H> is designed from the ground up to focus on "only bytes and references".

#### <LG>a</LG> <H>byte</H> <LG>is</LG> <i>8 bits</i>
#### <LG>a</LG> <H>ref</H> <LG>is</LG> <i>8 bytes</i>

### Bytes
If you're dealing with bytes, use:
<pre>
<Y>byte</Y>
<G>// byte x = '7';</G>
<G>// The '7' character is 55</G>
<G>// Which is 00110111 in bits</G>
</pre>

### Refs
A reference holds the location of something, rather than the thing itself:
<pre>
<LG>TYPE</LG> <Y>ref</Y> <LG>NAME</LG>
<G>// A reference to a TYPE</G>
</pre>

To get the reference for a declared variable:
<pre>
<M>ref_of</M><Y>(</Y> <LG>VAR</LG> <Y>)</Y>
<G>// Reference-of VAR</G>
</pre>

And to get the value that a reference points to:
<pre>
<M>val_of</M><Y>(</Y> <LG>REF</LG> <Y>)</Y>
<G>// Value-of REF</G>
</pre>

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

### Type Prefixes
<pre>
<Y>perm</Y> <LG>TYPE VAR</LG> <G>// VAR is permanent</G>
<G>// Made once, always exists in the scope</G>
<G>// Keeps its value between visits</G>

<Y>const</Y> <LG>TYPE NAME</LG> <G>// NAME is constant</G>
<G>// Cannot be changed!</G>

<Y>packed</Y> <G>// No padding between elements</G>
<G>// Used automatically by type/fusion/global</G>

<Y>cache_align</Y> <G>// Aligned to 64 bytes</G>
<G>// For things read very often</G>
</pre>
They can be combined:
<pre>
<Y>perm const</Y> <LG>TYPE NAME</LG>
<G>// A permanent constant TYPE</G>
<G>// perm always goes before const</G>
</pre>

### Mutation Permissions
Where you put <Y>const</Y> decides <b>what</b> is locked: the reference, or the value it points at.
<pre>
<Y>const</Y> <LG>TYPE</LG> <Y>ref const</Y> <LG>NAME</LG>
<G>// NAME and val_of NAME cannot change</G>

<LG>TYPE</LG> <Y>ref const</Y> <LG>NAME</LG>
<G>// NAME cannot change, but val_of NAME can</G>

<Y>const</Y> <LG>TYPE</LG> <Y>ref</Y> <LG>NAME</LG>
<G>// NAME can change, but val_of NAME cannot</G>

<LG>TYPE</LG> <Y>ref</Y> <LG>NAME</LG>
<G>// NAME and val_of NAME can change</G>
</pre>
<Y>const</Y> can be written before or after the type, so <Y>const</Y> <LG>TYPE</LG> and <LG>TYPE</LG> <Y>const</Y> mean the same thing.
<H>H</H> writes it after, so it reads left-to-right in the order it locks:
<pre>
<Y>byte const ref const</Y> <LG>NAME</LG>
<G>// "bytes that can't change,</G>
<G>//  at a place that can't change"</G>
</pre>

### Number Types
- <H>N</H>atural <LG>(cannot be less than zero)</LG>
	- <C>0, 1, 2, 3, ...</C>
- <H>I</H>nteger <LG>(can be negative)</LG>
	- <C>-3, -2, -1, 0, 1, 2, 3, ...</C>
- <H>R</H>ational <LG>(has a fractional part)</LG>
	- <C>-7./9., -1.5, 0.02, 1./2., 7./9., ...</C>

<pre>
<LG>[</LG><Y>n</Y><LG>/</LG><Y>i</Y><LG>/</LG><Y>r</Y><LG>][</LG><Y>size</Y><LG>]</LG>
<G>// Where [size] is in bytes</G>
</pre>

#### more <H>bytes</H> = more <H>range</H>

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
| <code><Y>r8</Y></code> | <b>8</b> | <b>-inf</b> <LG><i>to</i></LG> <b>inf</b> <LG>(more precise)</LG> |

Every type is also a converter, so <M>n1</M><Y>(</Y> <LG>x</LG> <Y>)</Y> is the same as <M>to</M><Y>(</Y> <Y>n1</Y><Y>,</Y> <LG>x</LG> <Y>)</Y>.
Each whole-number type carries its own limits:
<pre>
<C>n1_min_val</C> <G>// 0</G>
<C>n1_max_val</C> <G>// 255</G>
<C>i4_min_val</C> <G>// -2,147,483,648</G>
<C>i4_max_val</C> <G>// 2,147,483,647</G>
<G>// ...and so on for every n/i type</G>
</pre>

### Other Types
<pre>
<Y>flag</Y> <LG>VAR</LG>
<G>// A flag can either be:</G>
<C>yes</C> <G>// 1</G>
<G>// or</G>
<C>no</C>  <G>// 0</G>

<M>flag</M><Y>(</Y> <LG>VAL</LG> <Y>)</Y> <G>// Any value to yes/no</G>
<G>// Anything non-zero becomes yes</G>

<M>flip</M><Y>(</Y> <LG>FLAG</LG> <Y>)</Y> <G>// no to yes, yes to no</G>
<G>// flag is_ready = no;</G>
<G>// flip( is_ready );</G>
<G>// is_ready is now yes</G>

<M>pick</M><Y>(</Y> <LG>FLAG</LG><Y>,</Y> <LG>A</LG><Y>,</Y> <LG>B</LG> <Y>)</Y>
<G>// If FLAG is yes, pick A, else pick B</G>
<G>// Only the picked side is ever run</G>
</pre>

There's also an output-state type, used by <M>start</M> and by anything that can fail:
<pre>
<Y>out_state</Y>
<C>success</C> <G>// 0</G>
<C>failure</C> <G>// 1</G>
<C>warning</C> <G>// 2</G>
</pre>

### Bitfields
When a type element only needs a few bits, say how many values it holds and <H>H</H> works out the width:
<pre>
<M>bits</M><Y>(</Y> <LG>N</LG> <Y>)</Y>
<G>// Enough bits to hold 0 to N-1</G>
<G>// For indices, and group elements</G>

<M>bits_count</M><Y>(</Y> <LG>N</LG> <Y>)</Y>
<G>// Enough bits to hold 0 to N</G>
<G>// For counters that can reach N</G>

<M>bits_flag</M>
<G>// Exactly 1 bit, for a yes/no</G>

<G>// Used like:</G>
<M>type</M><Y>(</Y> <LG>entity</LG> <Y>)</Y>
<C>{</C>
	<Y>n1</Y> state <M>bits</M><Y>(</Y> entity_states_count <Y>)</Y><C>;</C>
	<Y>n1</Y> items_held <M>bits_count</M><Y>(</Y> <C>8</C> <Y>)</Y><C>;</C> <G>// 0 to 8</G>
	<Y>flag</Y> is_alive <M>bits_flag</M><C>;</C>
<C>};</C>
</pre>
<DG>Use bits_count for anything you add to, or it will wrap around to 0!</DG>

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

### Suffix Logic
<pre>
<M>is_even</M>
<G>// Check if number is even:</G>
<G>// if( x is_even )</G>
<G>// Same as: if( x mod 2 is 0 )</G>

<M>is_odd</M>
<G>// Check if number is odd:</G>
<G>// if( x is_odd )</G>
<G>// Same as: x mod 2 isnt 0</G>
</pre>

### Type Control
If you want to get the type of a value or variable, you can use:
<pre>
<M>type_of</M><Y>(</Y> <LG>VAR</LG> <Y>)</Y>
<G>// Deduces the type of VAR</G>
<G>// Which can be used in interesting ways:</G>
<Y>#define</Y> <M>SWAP</M><Y>(</Y> A<Y>,</Y> B <Y>)</Y><G>\</G>
	<M>START_DEF</M><G>\</G>
	<C>{</C><G>\</G>
		<M>type_of</M><Y>(</Y> A <Y>)</Y> _temp <Y>=</Y> A<C>;</C><G>\</G>
		A <Y>=</Y> B<C>;</C><G>\</G>
		B <Y>=</Y> _temp<C>;</C><G>\</G>
	<C>}</C><G>\</G>
	<M>END_DEF</M>
<G>// Which automatically deduces the type</G>
<G>//  for swapping any 2 same-type variables</G>

<M>type_of_ref</M><Y>(</Y> <LG>REF</LG> <Y>)</Y>
<G>// Deduces the type a reference points to</G>
</pre>

To convert a value into a different type, changing it to align with the type, use:
<pre>
<M>to</M><Y>(</Y> <LG>TYPE</LG><Y>,</Y> <LG>VAL</LG> <Y>)</Y>
<G>// Changes VAL to TYPE</G>

<G>// Can be used like:</G>
<Y>r4</Y> x <Y>=</Y> <C>-2.345;</C>
<Y>i2</Y> y <Y>=</Y> <M>to</M><Y>( i2,</Y> x <Y>)</Y><C>;</C>
<G>// y is now -2 (r* to i* truncates)</G>
</pre>

If you want to keep the bits the same, but change how it's read, you can use a reinterpret-cast:
<pre>
<M>cast</M><Y>(</Y> <LG>TYPE</LG><Y>,</Y> <LG>VAR</LG> <Y>)</Y>
<G>// Reinterpret VAR to TYPE</G>

<G>// Can be used like:</G>
<Y>r4</Y> x <Y>=</Y> <C>-2.345;</C>
<Y>i2</Y> y <Y>=</Y> <M>cast</M><Y>( i2,</Y> x <Y>)</Y><C>;</C>
<G>// y is now 5243 since the bits</G>
<G>//  are the same, but read differently</G>
</pre>

### Sizes
<pre>
<M>size_of</M><Y>(</Y> <LG>TYPE_OR_VAR</LG> <Y>)</Y>
<G>// How many bytes it takes up</G>

<M>size_of_bytes</M><Y>(</Y> <LG>BYTES</LG> <Y>)</Y>
<G>// Length of a "literal", without the '\0'</G>

<M>size_of_array</M><Y>(</Y> <LG>ARRAY</LG> <Y>)</Y>
<G>// How many elements are in an array</G>
</pre>

-------
## Defining New Types
Types can be an <H>alias</H> <LG>(a renamed type)</LG>,
<H>multi-typed</H> <LG>(contains elements)</LG>,
a <H>fusion</H> <LG>(elements use the same bytes)</LG>,
or a <H>group</H> <LG>(elements are the same type, and are ordinal)</LG>.

### Types
New types are made with one or more types:
<pre>
<M>type_from</M><Y>(</Y> <LG>TYPE</LG> <Y>)</Y> <LG>NAME</LG><C>;</C>
<G>// Defines an alias of TYPE, called NAME</G>

<M>type</M><Y>(</Y> <LG>NAME</LG> <Y>)</Y>
<C>{</C>
	<LG>TYPE NAME</LG><C>;</C>
	<LG>TYPE</LG> <Y>ref</Y> <LG>NAME</LG><C>;</C>
	<LG>TYPE</LG> <Y>const</Y> <LG>NAME</LG><C>;</C>
	<LG>...</LG>
<C>};</C> <DG><- the semicolon is required!</DG>
<G>// Defines a type made of multiple types</G>
</pre>

Accessing elements from a type:
<pre>
<M>type</M><Y>(</Y> <LG>NAME</LG> <Y>)</Y>
<C>{</C>
	<LG>TYPE A</LG><C>;</C>
	<LG>TYPE B</LG><C>;</C>
	<LG>...</LG>
<C>};</C>

<LG>NAME</LG> t<C>;</C>
t<C>.</C>A <Y>=</Y> <LG>...</LG>
<G>// If it's a type-value,</G>
<G>//  you use "</G><C>.</C><G>"</G>

<LG>NAME</LG> <Y>ref</Y> t_ref <Y>=</Y> <M>ref_of</M><Y>(</Y> t <Y>)</Y><C>;</C>
t_ref<C>-></C>B <Y>=</Y> <LG>...</LG>
<G>// If it's a type-reference,</G>
<G>//  you use "</G><C>-></C><G>"</G>
</pre>

To make a value of a type in one go, and to empty one:
<pre>
<M>make</M><Y>(</Y> <LG>TYPE</LG><Y>,</Y> <LG>ELEMENT_VALUES...</LG> <Y>)</Y>
<G>// A brand new TYPE value</G>
<G>// Elements can be given in order:</G>
<G>// make( point, 3, 7 )</G>
<G>// Or by name:</G>
<G>// make( point, .x = 3, .y = 7 )</G>
<G>// Anything left out becomes 0</G>

<M>wipe</M><Y>(</Y> <LG>VAR</LG> <Y>)</Y>
<G>// Sets every byte of VAR to 0</G>
</pre>

### Fusions
A fusion-type is always as big as the largest internal type, and all elements of a fusion use the same bytes:
<pre>
<M>fusion</M><Y>(</Y> <LG>NAME</LG> <Y>)</Y>
<C>{</C>
	<LG>TYPE A</LG><C>;</C>
	<LG>TYPE B</LG><C>;</C>
	<LG>...</LG>
<C>};</C> <DG><- the semicolon is required!</DG>
<G>// NAME has elements A, B, (etc...),</G>
<G>//  editing one edits the others</G>

<G>// You can make multi-type fusions with:</G>
<M>variant</M>
<G>// Which allows for element aliasing:</G>
<M>fusion</M><Y>(</Y> <LG>NAME</LG> <Y>)</Y>
<C>{</C>
	<M>variant</M>
	<C>{</C>
		<LG>TYPE X</LG><C>;</C>
		<LG>TYPE Y</LG><C>;</C>
		<LG>...</LG>
	<C>};</C>
	
	<M>variant</M>
	<C>{</C>
		<LG>TYPE W</LG><C>;</C>
		<LG>TYPE H</LG><C>;</C>
		<LG>...</LG>
	<C>};</C>
<C>};</C>
<G>// NAME.X and NAME.W read the same value,</G>
<G>//  same as NAME.Y and NAME.H</G>
<G>// Both variants take the same space</G>
</pre>

### Groups
A group allows you to define Natural/Integer constant-values under a name.
The group elements are separated by a <G>"</G><C>,</C><G>"</G> instead:
<pre>
<M>group</M><Y>(</Y> <LG>NAME</LG><Y>,</Y> <LG>OPTIONAL_TYPE</LG> <Y>)</Y>
<C>{</C>
	<LG>NAME_A</LG><C>,</C> <G>// starts at 0</G>
	<LG>NAME_B</LG><C>,</C> <G>// 1</G>
	<LG>NAME_C</LG><C>,</C> <G>// 2</G>
	<LG>...</LG>
<C>};</C> <DG><- the semicolon is required!</DG>
<G>// OPTIONAL_TYPE can be [n/i][1/2/4/8]</G>

<G>// You can explicitly define the value</G>
<G>//  if it's within the type-range:</G>
<M>group</M><Y>(</Y> <LG>NAME</LG><Y>, i2 )</Y>
<C>{</C>
	<LG>NAME_A</LG> <Y>=</Y> <C>-7,</C>
	<LG>NAME_B</LG><C>,</C> <G>// is -6</G>
	<LG>NAME_C</LG> <Y>=</Y> <C>777,</C>
	<LG>NAME_D</LG><C>,</C> <G>// is 778</G>
	<LG>...</LG>
<C>};</C>
</pre>

If there's going to be less than 256 elements, the default group-type is <Y>n1</Y>:
<pre>
<M>group</M><Y>(</Y> <LG>NAME</LG> <Y>)</Y>
<C>{</C>
	<LG>...</LG>
<C>};</C>
</pre>

A useful habit is to end a group with a count, so you always know how many there are:
<pre>
<M>group</M><Y>( entity_type )</Y>
<C>{</C>
	entity_player<C>,</C>
	entity_enemy<C>,</C>
	entity_projectile<C>,</C>
	<G>//</G>
	entity_types_count
<C>};</C>
<G>// entity_types_count is 3</G>
<G>// Which pairs with bits( entity_types_count )</G>
</pre>

The elements inside the group are available in the scope,
for example:
<pre>
<C>{</C> <G>// Some scope</G>
	<M>group</M><Y>( entity_type )</Y>
	<C>{</C>
		entity_player<C>,</C>
		entity_enemy<C>,</C>
		entity_projectile
	<C>};</C>
	<Y>entity_type</Y> t <Y>=</Y> entity_enemy<C>;</C>
<C>}</C>
<G>// entity_type and the elements are</G>
<G>//  not accessible outside the scope</G>
</pre>

### Globals
A global is a named collection that lives for the whole program, outside every scope.
<H>H</H> makes them explicit, because they're easy to overuse:
<pre>
<M>global</M>
<C>{</C>
	<Y>n4</Y> frame<C>;</C>
	<Y>r8</Y> delta_time<C>;</C>
	<Y>flag</Y> running<C>;</C>
<C>}</C>
<LG>program</LG><C>;</C>
<G>// Used anywhere as: program.frame</G>
</pre>

-------
## Execution Control
The program is "read" by the computer in a downwards flow, as if it's reading a book.
To move where it's "reading", you can use:
<H>skip</H> <LG>(skips the rest of the scope)</LG>,
<H>jump</H> <LG>(jumps to a POINT)</LG>,
<H>next</H> <LG>(next scope-iteration)</LG>,
or <H>out</H> <LG>(leaves the function)</LG>

### skip
When used inside a scope-structure like <H>loop</H>, <H>range</H>, <H>iter</H>, etc, it skips the code below it and the program goes to the point after the <G>"</G><C>}</C><G>"</G> of that scope-structure.

<H>skip</H> always exits the nearest scope-structure, regardless of how many <H>if</H> scopes it's nested within:
<pre>
<M>loop</M>
<C>{</C>
	<M>if</M><Y>(</Y> <LG>x</LG> <Y>)</Y>
	<C>{</C>
		<M>if</M><Y>(</Y> <LG>y</LG> <Y>)</Y>
		<C>{</C>
			<M>if</M><Y>(</Y> <LG>z</LG> <Y>)</Y>
			<C>{</C>
				<M>skip</M><C>;</C>
				<G>// Even 3 if()s deep,</G>
				<G>//  skip exits the loop</G>
			<C>}</C>
		<C>}</C>
	<C>}</C>
	<LG>...</LG>
<C>}</C>
<G>// The skip takes us here</G>
</pre>

When scope-structures are nested, <H>skip</H> only exits the immediate scope-structure it's in:
<pre>
<M>iter</M><Y>(</Y> i<Y>,</Y> <C>10</C> <Y>)</Y>
<C>{</C>
	<M>iter</M><Y>(</Y> j<Y>,</Y> <C>10</C> <Y>)</Y>
	<C>{</C>
		<M>if</M><Y>(</Y> j <Y>></Y> <C>5</C> <Y>)</Y>
		<C>{</C>
			<M>skip</M><C>;</C>
			<G>// Only exits the inner iter</G>
		<C>}</C>
		<LG>...</LG>
	<C>}</C>
	<G>// Skip takes us here,</G>
	<G>//  still in iter( i, 10 )</G>
	<LG>...</LG>
<C>}</C>
<G>// To get here, you'd need another skip</G>
<G>//  in iter( i, 10 ), or when it's done</G>
</pre>

### jump
The program can jump to any point within the same function.
So the name of the point cannot be repeated in function it's defined in, and jumping can only occur in the function.

Jumping <b>backwards</b> is the typical use-case, since jumping forwards can potentially skip important code.
<pre>
<LG>POINT</LG><C>:</C> <G>// Set a point in the code</G>

<M>jump</M> <LG>POINT</LG><C>;</C> <G>// Jump to POINT</G>
<G>// Jumps immediately to where POINT: is</G>
<G>//  (must be in the same function)</G>

<G>// Example of backwards jumping:</G>
<Y>do_thing</Y><C>:</C>
<C>{</C>
	<LG>...</LG>
	<M>if</M><Y>(</Y> <LG>FLAG</LG> <Y>)</Y>
	<C>{</C>
		<LG>...</LG>
		<M>jump</M> <Y>thing_finished</Y><C>;</C>
	<C>}</C>
	
	<M>jump</M> <Y>do_thing</Y><C>;</C> <G>// Loop back</G>
<C>}</C>
<Y>thing_finished</Y><C>:</C>
</pre>

To exit multiple nested scope-structures at once, use <H>jump</H> with a label:
<pre>
<M>iter</M><Y>(</Y> i<Y>,</Y> <C>10</C> <Y>)</Y>
<C>{</C>
	<M>iter</M><Y>(</Y> j<Y>,</Y> <C>10</C> <Y>)</Y>
	<C>{</C>
		<M>if</M><Y>(</Y> <LG>FLAG</LG> <Y>)</Y>
		<C>{</C>
			<M>jump</M> <Y>this_done</Y><C>;</C>
			<G>// Exits both iters</G>
		<C>}</C>
	<C>}</C>
<C>}</C>
<Y>this_done</Y><C>:</C>
<G>// Jump brings us here</G>
</pre>

### next
In iteration-scopes you often want to have control as to when it jumps to the next iteration:
<pre>
<M>next</M> <G>// Jump up to next iteration</G>
<G>// "skip the rest, go to the next", like:</G>
<M>while</M><Y>(</Y> x <Y><</Y> <C>10</C> <Y>)</Y>
<C>{</C>
	x <Y>=</Y> x <Y>+</Y> <C>1;</C>
	<M>if</M><Y>(</Y> x <Y><</Y> <C>5</C> <Y>)</Y>
	<C>{</C>
		<M>next</M><C>;</C>
		<G>// jumps back up to the while</G>
	<C>}</C>
	<G>// This code is ran if x >= 5:</G>
	<LG>...</LG>
<C>}</C>
</pre>

### Conditional Forms
Every one of these has <LG>*</LG><M>_if</M> forms that are often easier to read:
<pre>
<M>skip_if</M><Y>(</Y> <LG>FLAG</LG> <Y>)</Y><C>;</C>
<G>// Skip if FLAG is yes</G>

<M>next_if</M><Y>(</Y> <LG>FLAG</LG> <Y>)</Y><C>;</C>
<G>// Jump to next iteration if FLAG is yes</G>

<M>jump_if</M><Y>(</Y> <LG>FLAG</LG> <Y>)</Y> <LG>POINT</LG><C>;</C>
<G>// Jump to POINT if FLAG is yes</G>

<M>out_if</M><Y>(</Y> <LG>FLAG</LG> <Y>)</Y> <LG>VAL</LG><C>;</C>
<G>// Output VAL if FLAG is yes</G>
<G>// Can be empty if fn doesn't output:</G>
<G>// out_if( FLAG );</G>
</pre>

And each of those four has the same set of endings, so you can say exactly what you mean:

| Ending | Checks |
|--------|--------|
| <code><M>_if</M><Y>(</Y> <LG>A</LG> <Y>)</Y></code> | A is yes |
| <code><M>_if_nothing</M><Y>(</Y> <LG>REF</LG> <Y>)</Y></code> | REF is nothing |
| <code><M>_if_something</M><Y>(</Y> <LG>REF</LG> <Y>)</Y></code> | REF isnt nothing |
| <code><M>_if_any</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>...</LG> <Y>)</Y></code> | any of them are yes |
| <code><M>_if_all</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>...</LG> <Y>)</Y></code> | all of them are yes |
| <code><M>_if_none</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>...</LG> <Y>)</Y></code> | none of them are yes |
| <code><M>_if_not_all</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>...</LG> <Y>)</Y></code> | not all of them are yes |

<pre>
<G>// So all of these exist:</G>
<M>skip_if_nothing</M><Y>(</Y> <LG>REF</LG> <Y>)</Y><C>;</C>
<M>next_if_any</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG> <Y>)</Y><C>;</C>
<M>out_if_all</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG> <Y>)</Y> <LG>VAL</LG><C>;</C>
<M>jump_if_something</M><Y>(</Y> <LG>REF</LG> <Y>)</Y> <LG>POINT</LG><C>;</C>
<LG>...</LG>
</pre>

-------
## Scope Structures

### with/when Structures
<pre>
<G>// Jump to a when() depending on what it is</G>
<M>with</M><Y>(</Y> <LG>VAL</LG> <Y>)</Y>
<C>{</C>
	<M>when</M><Y>(</Y> <C>1</C><Y>,</Y> <C>2</C><Y>,</Y> <C>3</C> <Y>)</Y>
	<C>{</C>
		<G>// Code only if VAL is 1, 2, or 3</G>
	<C>}</C>
	
	<M>when</M><Y>(</Y> <C>4</C> <Y>)</Y>
	<C>{</C>
		<G>// Code specifically for if VAL is 4</G>
	<C>}</C>
	
	<M>other</M>
	<C>{</C>
		<G>// Code for anything else</G>
	<C>}</C>
<C>}</C>
<G>// Each when ends by itself,</G>
<G>//  it won't fall into the next one</G>
</pre>

If you <b>want</b> one to continue into the next, use the <M>then_</M> forms:
<pre>
<M>with</M><Y>(</Y> <LG>VAL</LG> <Y>)</Y>
<C>{</C>
	<M>when</M><Y>(</Y> <C>1</C> <Y>)</Y>
	<C>{</C>
		<G>// Runs for 1...</G>
	<C>}</C>
	<M>then_when</M><Y>(</Y> <C>2</C> <Y>)</Y>
	<C>{</C>
		<G>// ...and continues into here</G>
		<G>// Also runs on its own for 2</G>
	<C>}</C>
	
	<M>then_other</M>
	<C>{</C>
		<G>// ...and continues into here too</G>
	<C>}</C>
<C>}</C>
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

<M>any</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>C</LG><Y>,</Y> <LG>...</LG> <Y>)</Y>
<G>// ( A or B or C ... )</G>

<M>all</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>C</LG><Y>,</Y> <LG>...</LG> <Y>)</Y>
<G>// ( A and B and C ... )</G>

<M>none</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>C</LG><Y>,</Y> <LG>...</LG> <Y>)</Y>
<G>// ( not ( A or B or C ... ) )</G>

<M>not_all</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>C</LG><Y>,</Y> <LG>...</LG> <Y>)</Y>
<G>// ( not ( A and B and C ... ) )</G>

<M>if_any</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>C</LG><Y>,</Y> <LG>...</LG> <Y>)</Y>
<C>{</C>
	<G>// If any of the inputs are yes</G>
<C>}</C>

<M>if_all</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>C</LG><Y>,</Y> <LG>...</LG> <Y>)</Y>
<C>{</C>
	<G>// If all of the inputs are yes</G>
<C>}</C>

<M>if_none</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>C</LG><Y>,</Y> <LG>...</LG> <Y>)</Y>
<C>{</C>
	<G>// If none of the inputs are yes</G>
<C>}</C>

<M>if_not_all</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>C</LG><Y>,</Y> <LG>...</LG> <Y>)</Y>
<C>{</C>
	<G>// If not all of the inputs are yes</G>
<C>}</C>
</pre>

### Loop Scopes
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
<M>while</M><Y>(</Y> <LG>FLAG</LG> <Y>)</Y><C>;</C>

<M>do</M>
<C>{</C>
	<G>// Run this code</G>
	<G>// Until FLAG is yes</G>
<C>}</C>
<M>until</M><Y>(</Y> <LG>FLAG</LG> <Y>)</Y><C>;</C>
</pre>

<M>while</M> also takes the multi-input forms:
<pre>
<M>while_any</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG> <Y>)</Y>
<M>while_all</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG> <Y>)</Y>
<M>while_none</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG> <Y>)</Y>
<M>while_not_all</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG> <Y>)</Y>
</pre>

### Iteration Scopes

<pre>
<G>// Range functions include from and to</G>
<M>range</M><Y>(</Y> <LG>VAR</LG><Y>,</Y> <LG>FROM</LG><Y>,</Y> <LG>TO</LG> <Y>)</Y>
<C>{</C>
	<G>// Iterates VAR in a FROM-TO range</G>
	<G>// Progresses 1 at a time</G>
	<G>// ( i, 2, 7 ) makes i go from 2 to 7</G>
<C>}</C>

<M>range_step</M><Y>(</Y> <LG>VAR</LG><Y>,</Y> <LG>FROM</LG><Y>,</Y> <LG>TO</LG><Y>,</Y> <LG>STEP</LG> <Y>)</Y>
<C>{</C>
	<G>// Iterates VAR in a FROM-TO range,</G>
	<G>//  but progresses with STEP</G>
<C>}</C>

<M>range_inv</M><Y>(</Y> <LG>VAR</LG><Y>,</Y> <LG>FROM</LG><Y>,</Y> <LG>TO</LG> <Y>)</Y>
<M>range_step_inv</M><Y>(</Y> <LG>VAR</LG><Y>,</Y> <LG>FROM</LG><Y>,</Y> <LG>TO</LG><Y>,</Y> <LG>STEP</LG> <Y>)</Y>
<C>{</C>
	<G>// The same, but counting downwards</G>
<C>}</C>

<G>// Iter functions always start from 0</G>
<M>iter</M><Y>(</Y> <LG>VAR</LG><Y>,</Y> <LG>SIZE</LG> <Y>)</Y>
<C>{</C>
	<G>// Iterates VAR from 0 to SIZE-1</G>
	<G>// Progresses 1 at a time</G>
<C>}</C>

<M>iter_step</M><Y>(</Y> <LG>VAR</LG><Y>,</Y> <LG>SIZE</LG><Y>,</Y> <LG>STEP</LG> <Y>)</Y>
<C>{</C>
	<G>// Iterates VAR from 0 to SIZE-1,</G>
	<G>//  but progresses with STEP</G>
<C>}</C>

<M>iter_inv</M><Y>(</Y> <LG>VAR</LG><Y>,</Y> <LG>SIZE</LG> <Y>)</Y>
<M>iter_step_inv</M><Y>(</Y> <LG>VAR</LG><Y>,</Y> <LG>SIZE</LG><Y>,</Y> <LG>STEP</LG> <Y>)</Y>
<C>{</C>
	<G>// The same, but from SIZE-1 down to 0</G>
<C>}</C>

<M>iter_grid</M><Y>(</Y> <LG>X</LG><Y>,</Y> <LG>Y</LG><Y>,</Y> <LG>WIDTH</LG><Y>,</Y> <LG>HEIGHT</LG> <Y>)</Y>
<C>{</C>
	<G>// Iterates X from 0 to WIDTH-1,</G>
	<G>//  and Y from 0 to HEIGHT-1</G>
	<G>// Left-to-right, top-to-bottom</G>
	<G>// For things like pixel images</G>
<C>}</C>

<M>range_grid</M><Y>(</Y> <LG>X</LG><Y>,</Y> <LG>Y</LG><Y>,</Y> <LG>X_FROM</LG><Y>,</Y> <LG>X_TO</LG><Y>,</Y> <LG>Y_FROM</LG><Y>,</Y> <LG>Y_TO</LG> <Y>)</Y>
<C>{</C>
	<G>// The same, but over a sub-area</G>
<C>}</C>

<M>repeat</M><Y>(</Y> <LG>N</LG> <Y>)</Y>
<C>{</C>
	<G>// Repeats this scope N-times</G>
	<G>// No counter variable is made</G>
<C>}</C>

<M>once</M>
<C>{</C>
	<G>// This scope runs only once</G>
	<G>// Even if it's visited again</G>
	<G>// Used for testing or initializing</G>
<C>}</C>
</pre>

-------
## Functions

### Function Declaration
Functions can only be created at the global-scope, outside of any other function/scope.

If the function doesn't output anything:
<pre>
<M>fn</M> <LG>NAME</LG><Y>(</Y> <LG>TYPE A</LG><Y>,</Y> <LG>TYPE B</LG><Y>,</Y> <LG>TYPE C</LG><Y>,</Y> <LG>...</LG> <Y>)</Y>
<C>{</C>
	<G>// function code</G>
	<M>out</M><C>;</C> <G>// exits the function</G>
	<G>// out is OPTIONAL</G>
<C>}</C> <G>// The program will jump back when done</G>
</pre>

If the function does output something:
<pre>
<LG>TYPE</LG> <LG>NAME</LG><Y>(</Y> <LG>TYPE A</LG><Y>,</Y> <LG>TYPE B</LG><Y>,</Y> <LG>TYPE C</LG><Y>,</Y> <LG>...</LG> <Y>)</Y>
<C>{</C>
	<G>// function code</G>
	<M>out</M> <LG>VAL</LG><C>;</C> <G>// outputs a value of TYPE</G>
<C>}</C>
</pre>

### Usage Examples
<pre>
<G>// Define a function:</G>
<Y>i4</Y> <M>add</M><Y>( i4</Y> x<Y>, i4</Y> y <Y>)</Y>
<C>{</C>
	<M>out</M> x <Y>+</Y> y<C>;</C>
<C>}</C>

<C>{</C><G>// Calling it in some scope:</G>
	<Y>i4</Y> result <Y>=</Y> <M>add</M><Y>(</Y> <C>3</C><Y>,</Y> <C>5</C> <Y>)</Y><C>;</C> <G>// result is 8</G>
<C>}</C>

<G>// Functions without output:</G>
<M>fn</M> <M>greet</M><Y>( byte const ref const</Y> name <Y>)</Y>
<C>{</C>
	<M>print</M><Y>(</Y> <C>"Hi "</C> <Y>)</Y><C>;</C>
	<M>print</M><Y>(</Y> name <Y>)</Y><C>;</C>
<C>}</C>

<C>{</C> <G>// In some scope:</G>
	<M>greet</M><Y>(</Y> <C>"Britta"</C> <Y>)</Y><C>;</C> <G>// Prints: Hi Britta</G>
<C>}</C>
</pre>

### Embed Function Prefix
<pre>
<M>embed</M> <LG>TYPE</LG> <LG>NAME</LG><Y>(</Y> <LG>...</LG> <Y>)</Y>
<G>// This will force the compiler to embed</G>
<G>//  the function code in where it's called</G>
<G>// No jump, no return: it becomes part</G>
<G>//  of the code that called it</G>

<M>embed</M> <Y>anon</Y> <LG>NAME</LG><Y>(</Y> <LG>...</LG> <Y>)</Y>
<G>// An embedded function with no output</G>
</pre>

### Function References
A function reference holds "which function to call", so it can be swapped at runtime:
<pre>
<M>fn_ref</M><Y>(</Y> <LG>OUTPUT</LG><Y>,</Y> <LG>NAME</LG><Y>,</Y> <LG>INPUT_TYPES...</LG> <Y>)</Y>
<G>// Declares a function reference called NAME</G>
<G>// fn_ref( i4, on_score, i4, i4 );</G>
<G>// on_score = add;</G>
<G>// i4 x = on_score( 3, 5 );</G>

<M>type_fn</M><Y>(</Y> <LG>OUTPUT</LG><Y>,</Y> <LG>INPUT_TYPES...</LG> <Y>)</Y> <LG>NAME</LG><C>;</C>
<G>// Makes a function-reference TYPE</G>
<G>// type_fn( anon, entity ref const ) entity_fn;</G>
<G>// Then used as: entity_fn on_tick;</G>

<M>call</M><Y>(</Y> <LG>FN</LG><Y>,</Y> <LG>INPUTS...</LG> <Y>)</Y><C>;</C>
<G>// Calls FN only if it isnt nothing</G>
<G>// Saves writing an if_something every time</G>
</pre>

-------
## Objects
<H>H</H> has no <i>object</i> keyword. An object is just a <H>type</H>, a <H>ref</H> to it, and functions that take that ref.
Naming the functions after the type keeps them together, and reads as a sentence:
<pre>
<M>type</M><Y>(</Y> <LG>player</LG> <Y>)</Y>
<C>{</C>
	<Y>i2</Y> health<C>;</C>
	<Y>r8</Y> x<C>;</C>
	<Y>r8</Y> y<C>;</C>
<C>};</C>

<M>fn</M> <M>player_ref_move</M><Y>(</Y> <Y>player ref const</Y> player_ref<Y>, r8 const</Y> x<Y>, r8 const</Y> y <Y>)</Y>
<C>{</C>
	player_ref<C>-></C>x <Y>+=</Y> x<C>;</C>
	player_ref<C>-></C>y <Y>+=</Y> y<C>;</C>
<C>}</C>
<G>// "player ref const" means the function</G>
<G>//  can edit the player, but cannot be</G>
<G>//  pointed at a different one</G>
</pre>

Used like:
<pre>
<C>{</C> <G>// In a scope somewhere</G>
	<Y>player ref</Y> main_player <Y>=</Y> <M>os_create_ref</M><Y>(</Y> <LG>player</LG> <Y>)</Y><C>;</C>
	main_player<C>-></C>health <Y>=</Y> <C>100;</C>
	main_player<C>-></C>x <Y>=</Y> <C>-20.5;</C>
	main_player<C>-></C>y <Y>=</Y> <C>77.0;</C>
	
	<M>player_ref_move</M><Y>(</Y> main_player<C>, 1.25, -5.1</C> <Y>)</Y><C>;</C>
	<G>// main_player->x is now -19.25, and</G>
	<G>// main_player->y is now 71.9</G>
	
	<M>os_delete_ref</M><Y>(</Y> main_player <Y>)</Y><C>;</C>
<C>}</C>
</pre>

If it only needs to exist inside one scope, you don't need a ref at all:
<pre>
<C>{</C>
	<Y>player</Y> p <Y>=</Y> <M>make</M><Y>(</Y> <LG>player</LG><Y>,</Y> <C>100</C><Y>,</Y> <C>-20.5</C><Y>,</Y> <C>77.0</C> <Y>)</Y><C>;</C>
	<M>player_ref_move</M><Y>(</Y> <M>ref_of</M><Y>(</Y> p <Y>),</Y> <C>1.25</C><Y>,</Y> <C>-5.1</C> <Y>)</Y><C>;</C>
<C>}</C> <G>// p disappears here, nothing to delete</G>
</pre>

-------
## Memory Operations

### Dynamic Memory
Memory from the operating system lives until you delete it, so it survives scopes.
<H>H</H> asks the OS for whole pages directly, and remembers the size for you.
<pre>
<M>os_create_ref</M><Y>(</Y> <LG>TYPE</LG><Y>,</Y> <LG>OPTIONAL_AMOUNT</LG> <Y>)</Y>
<G>// Makes room for AMOUNT of TYPE</G>
<G>// OPTIONAL_AMOUNT is 1 by default</G>
<G>// Memory is zero-filled</G>
<G>// Outputs nothing if it failed</G>

<M>os_resize_ref</M><Y>(</Y> <LG>REF</LG><Y>,</Y> <LG>AMOUNT</LG><Y>,</Y> <LG>OPTIONAL_PRESERVE</LG> <Y>)</Y>
<G>// Resizes REF to hold AMOUNT elements</G>
<G>// AMOUNT is counted the same way</G>
<G>//  os_create_ref counts it</G>
<G>// OPTIONAL_PRESERVE is no by default:</G>
<G>//  yes keeps the old contents</G>

<M>os_delete_ref</M><Y>(</Y> <LG>REF</LG> <Y>)</Y><C>;</C>
<G>// Gives the memory back</G>
<G>// Sets REF to nothing afterwards,</G>
<G>//  so it can't be used by accident</G>

<G>// Used like:</G>
<Y>n4 ref</Y> scores <Y>=</Y> <M>os_create_ref</M><Y>( n4,</Y> <C>100</C> <Y>)</Y><C>;</C>
scores <Y>=</Y> <M>os_resize_ref</M><Y>(</Y> scores<Y>,</Y> <C>200</C><Y>,</Y> <C>yes</C> <Y>)</Y><C>;</C>
<M>os_delete_ref</M><Y>(</Y> scores <Y>)</Y><C>;</C>
</pre>

### Byte Operations
Every one of these is written <b>to-first</b>, the same order as <Y>=</Y> :
what you're writing into comes before what you're reading from.
<pre>
<M>bytes_copy</M><Y>(</Y> <LG>TO</LG><Y>,</Y> <LG>FROM</LG><Y>,</Y> <LG>SIZE</LG> <Y>)</Y>
<G>// Copy SIZE bytes from FROM into TO</G>

<M>bytes_copy_until</M><Y>(</Y> <LG>TO</LG><Y>,</Y> <LG>FROM</LG><Y>,</Y> <LG>BYTE</LG><Y>,</Y> <LG>MAX_SIZE</LG> <Y>)</Y>
<G>// Copy until BYTE is found, or MAX_SIZE</G>

<M>bytes_copy_until_any</M><Y>(</Y> <LG>TO</LG><Y>,</Y> <LG>FROM</LG><Y>,</Y> <LG>DELIMITERS</LG> <Y>)</Y>
<G>// Copy until any byte in DELIMITERS</G>

<M>bytes_move</M><Y>(</Y> <LG>REF</LG><Y>,</Y> <LG>POSITION</LG><Y>,</Y> <LG>SIZE</LG><Y>,</Y> <LG>AMOUNT</LG> <Y>)</Y>
<G>// Shift SIZE bytes at POSITION</G>
<G>//  along by AMOUNT, within the same REF</G>
<G>// Safe when the two areas overlap</G>

<M>bytes_fill</M><Y>(</Y> <LG>REF</LG><Y>,</Y> <LG>VAL</LG><Y>,</Y> <LG>SIZE</LG> <Y>)</Y>
<G>// Fill SIZE bytes with VAL</G>

<M>bytes_clear</M><Y>(</Y> <LG>REF</LG><Y>,</Y> <LG>SIZE</LG> <Y>)</Y>
<G>// Fill SIZE bytes with 0</G>

<M>bytes_compare</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>OPTIONAL_SIZE</LG> <Y>)</Y>
<G>// Outputs 0 if equal,</G>
<G>//  negative if A < B, positive if A > B</G>
<G>// OPTIONAL_SIZE measures B by default</G>

<M>bytes_match</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>OPTIONAL_SIZE</LG> <Y>)</Y>
<G>// yes if they're the same</G>
</pre>

### Null-Terminated Operations
Bytes that end in a <C>'\0'</C> can measure themselves:
<pre>
<M>bytes_measure</M><Y>(</Y> <LG>REF</LG> <Y>)</Y>
<G>// Count bytes until '\0'</G>

<M>bytes_paste</M><Y>(</Y> <LG>TO</LG><Y>,</Y> <LG>FROM</LG><Y>,</Y> <LG>OPTIONAL_SIZE</LG> <Y>)</Y>
<G>// Copy FROM into TO, including the '\0'</G>
<G>// OPTIONAL_SIZE measures FROM by default</G>

<M>bytes_find</M><Y>(</Y> <LG>BYTES</LG><Y>,</Y> <LG>BYTE</LG><Y>,</Y> <LG>MAX_SIZE</LG> <Y>)</Y>
<G>// Outputs a ref to the first BYTE found,</G>
<G>//  or nothing</G>

<M>bytes_find_bytes</M><Y>(</Y> <LG>BYTES</LG><Y>,</Y> <LG>SIZE</LG><Y>,</Y> <LG>TARGET</LG><Y>,</Y> <LG>OPTIONAL_SIZE</LG> <Y>)</Y>
<G>// Outputs a ref to the first TARGET found,</G>
<G>//  or nothing</G>

<M>bytes_end</M><Y>(</Y> <LG>REF</LG> <Y>)</Y>
<G>// Set the byte at REF to '\0'</G>
<G>// Doesn't measure REF!</G>
</pre>

### Moving Operations
These write, then push the <LG>TO</LG> reference forward, so you can lay bytes down one after another:
<pre>
<M>bytes_copy_move</M><Y>(</Y> <LG>TO</LG><Y>,</Y> <LG>FROM</LG><Y>,</Y> <LG>SIZE</LG> <Y>)</Y>
<G>// Copy SIZE bytes, then move TO by SIZE</G>

<M>bytes_paste_move</M><Y>(</Y> <LG>TO</LG><Y>,</Y> <LG>FROM</LG> <Y>)</Y>
<G>// Paste FROM, then move TO to the '\0'</G>
<G>// So the next paste writes over it</G>

<M>bytes_set_move</M><Y>(</Y> <LG>TO</LG><Y>,</Y> <LG>BYTE</LG> <Y>)</Y>
<G>// Set one byte, then move TO by 1</G>

<M>bytes_newline_move</M><Y>(</Y> <LG>TO</LG> <Y>)</Y>
<M>bytes_separator_move</M><Y>(</Y> <LG>TO</LG> <Y>)</Y>
<G>// Set a '\n' or a path separator, and move</G>

<G>// Used like:</G>
<Y>byte</Y> message<Y>[</Y> <C>64</C> <Y>]</Y><C>;</C>
<Y>byte ref</Y> message_ref <Y>=</Y> message<C>;</C>
<M>bytes_paste_move</M><Y>(</Y> message_ref<Y>,</Y> <C>"Hi "</C> <Y>)</Y><C>;</C>
<M>bytes_paste_move</M><Y>(</Y> message_ref<Y>,</Y> <C>"Britta"</C> <Y>)</Y><C>;</C>
<M>bytes_newline_move</M><Y>(</Y> message_ref <Y>)</Y><C>;</C>
<M>bytes_end</M><Y>(</Y> message_ref <Y>)</Y><C>;</C>
<G>// message is now "Hi Britta\n"</G>
</pre>

### Byte Literals
<pre>
<C>newline</C> <G>// "\n"</G>
<C>tab</C>     <G>// "\t"</G>
<C>eof</C>     <G>// "\0"</G>
<C>separator</C> <G>// "/" or "\\"</G>

<G>// Each has a single-byte form:</G>
<C>newline_byte</C> <C>tab_byte</C> <C>eof_byte</C> <C>separator_byte</C>
</pre>

### Character Checks
<pre>
<M>is_letter</M><Y>(</Y> <LG>BYTE</LG> <Y>)</Y> <G>// yes if a-z or A-Z</G>
<M>is_number</M><Y>(</Y> <LG>BYTE</LG> <Y>)</Y> <G>// yes if 0-9</G>

<M>to_upper_case</M><Y>(</Y> <LG>BYTE</LG> <Y>)</Y> <G>// 'h' to 'H'</G>
<M>to_lower_case</M><Y>(</Y> <LG>BYTE</LG> <Y>)</Y> <G>// 'H' to 'h'</G>
<G>// Anything that isn't a letter is untouched</G>
</pre>

-------
## Numbers to Bytes
Turning a number into readable text has one shape, for every type and every base:
<pre>
<LG>TYPE</LG><M>_to_bytes</M><Y>(</Y> <LG>VALUE</LG><Y>,</Y> <LG>TO</LG> <Y>)</Y>
<G>// Writes VALUE at TO, TO stays put</G>

<LG>TYPE</LG><M>_to_bytes_move</M><Y>(</Y> <LG>VALUE</LG><Y>,</Y> <LG>TO</LG> <Y>)</Y>
<G>// Writes VALUE at TO, then moves TO</G>
<G>//  past what it wrote</G>
</pre>

| Family | Available for | Writes |
|--------|---------------|--------|
| <code><M>n1_to_bytes</M></code> <LG>...</LG> <code><M>n8_to_bytes</M></code> | n1 n2 n4 n8 | <b>0</b> <LG>to</LG> <b>18446744073709551615</b> |
| <code><M>i1_to_bytes</M></code> <LG>...</LG> <code><M>i8_to_bytes</M></code> | i1 i2 i4 i8 | with a <b>-</b> if negative |
| <code><M>r4_to_bytes</M></code> <LG>,</LG> <code><M>r8_to_bytes</M></code> | r4 r8 | <b>12.3400</b> <LG>(4 or 8 decimals)</LG> |
| <code><M>octal_n1_to_bytes</M></code> <LG>...</LG> | n1 n2 n4 n8 | base <b>8</b> |
| <code><M>hex_n1_to_bytes</M></code> <LG>...</LG> | n1 n2 n4 n8 | base <b>16</b>, <b>0-9 A-F</b> |

<pre>
<G>// Used like:</G>
<Y>byte</Y> line<Y>[</Y> <C>64</C> <Y>]</Y><C>;</C>
<Y>byte ref</Y> line_ref <Y>=</Y> line<C>;</C>

<M>bytes_paste_move</M><Y>(</Y> line_ref<Y>,</Y> <C>"score: "</C> <Y>)</Y><C>;</C>
<M>n4_to_bytes_move</M><Y>(</Y> <C>1250</C><Y>,</Y> line_ref <Y>)</Y><C>;</C>
<M>bytes_paste_move</M><Y>(</Y> line_ref<Y>,</Y> <C>" (0x"</C> <Y>)</Y><C>;</C>
<M>hex_n4_to_bytes_move</M><Y>(</Y> <C>1250</C><Y>,</Y> line_ref <Y>)</Y><C>;</C>
<M>bytes_paste_move</M><Y>(</Y> line_ref<Y>,</Y> <C>")"</C> <Y>)</Y><C>;</C>
<M>bytes_end</M><Y>(</Y> line_ref <Y>)</Y><C>;</C>
<G>// line is now "score: 1250 (0x4E2)"</G>
</pre>

There's also one for turning raw bytes into a text-literal you can paste into code:
<pre>
<M>bytes_to_h</M><Y>(</Y> <LG>IN_BYTES</LG><Y>,</Y> <LG>IN_SIZE</LG><Y>,</Y> <LG>OUT_REF</LG> <Y>)</Y><C>;</C>
<G>// Escapes quotes, slashes, newlines, etc.</G>
<G>// OUT_REF is a ref to a byte ref,</G>
<G>//  and it gets moved as it writes</G>
<G>// Used for baking files into source</G>
</pre>

-------
## Math Operations

### Math Macros
These work on any type, and are written in CAPITALS because they're direct text replacements:
<pre>
<M>MIN</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG> <Y>)</Y>       <G>// Smaller of the two</G>
<M>MIN3</M> <M>MIN4</M> <M>MIN5</M>   <G>// Smallest of 3, 4, or 5</G>

<M>MAX</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG> <Y>)</Y>       <G>// Larger of the two</G>
<M>MAX3</M> <M>MAX4</M> <M>MAX5</M>   <G>// Largest of 3, 4, or 5</G>

<M>MEDIAN</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>C</LG> <Y>)</Y>
<M>MEDIAN4</M> <M>MEDIAN5</M> <G>// Middle value</G>

<M>AVG</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG> <Y>)</Y>       <G>// Average of the two</G>
<M>AVG3</M> <M>AVG4</M>          <G>// Average of 3 or 4</G>

<M>CLAMP</M><Y>(</Y> <LG>VAL</LG><Y>,</Y> <LG>MIN</LG><Y>,</Y> <LG>MAX</LG> <Y>)</Y>
<G>// Keep VAL between MIN and MAX</G>

<M>SATURATE</M><Y>(</Y> <LG>VAL</LG> <Y>)</Y> <G>// Keep between 0 and 1</G>

<M>ABS</M><Y>(</Y> <LG>VAL</LG> <Y>)</Y>       <G>// Remove negative sign</G>
<M>SIGN</M><Y>(</Y> <LG>VAL</LG> <Y>)</Y>      <G>// 1 if positive or 0, -1 if negative</G>
<M>SIGN_ZERO</M><Y>(</Y> <LG>VAL</LG> <Y>)</Y> <G>// 0 if 0, 1 if positive, -1 if negative</G>

<M>SQR</M><Y>(</Y> <LG>VAL</LG> <Y>)</Y>       <G>// VAL * VAL</G>
<M>CUBE</M><Y>(</Y> <LG>VAL</LG> <Y>)</Y>      <G>// VAL * VAL * VAL</G>

<M>MIX</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>AMOUNT</LG> <Y>)</Y>
<G>// Blend from A to B, 0.0 to 1.0</G>

<M>MAP</M><Y>(</Y> <LG>VAL</LG><Y>,</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>C</LG><Y>,</Y> <LG>D</LG> <Y>)</Y>
<G>// Move VAL from the A-B range</G>
<G>//  into the C-D range</G>

<M>RANGE</M><Y>(</Y> <LG>VAL</LG><Y>,</Y> <LG>LOWER</LG><Y>,</Y> <LG>UPPER</LG> <Y>)</Y>
<G>// Where VAL sits between them, as 0.0-1.0</G>
</pre>
<DG>These write their inputs out more than once, so avoid MAX( x++, y )</DG>

The <M>_BITWISE</M> forms divide by shifting instead, which is faster but only correct on whole numbers:
<pre>
<M>AVG_BITWISE</M> <M>AVG4_BITWISE</M> <M>MEDIAN4_BITWISE</M>
</pre>

### Typed Math Functions
Every number type also has real functions, which only read their inputs once.
They're named <LG>type</LG><M>_operation</M>, so the type you're working in is always visible:
<pre>
<Y>i4</Y> x <Y>=</Y> <M>i4_clamp</M><Y>(</Y> value<Y>,</Y> <C>0</C><Y>,</Y> <C>100</C> <Y>)</Y><C>;</C>
<Y>r4</Y> y <Y>=</Y> <M>r4_mix</M><Y>(</Y> start<Y>,</Y> end<Y>,</Y> <C>0.25</C> <Y>)</Y><C>;</C>
<Y>n2</Y> z <Y>=</Y> <M>n2_min3</M><Y>(</Y> a<Y>,</Y> b<Y>,</Y> c <Y>)</Y><C>;</C>
</pre>

| Operations | Available on |
|------------|--------------|
| <code><M>_min</M> <M>_min3</M> <M>_min4</M> <M>_min5</M></code> | every type |
| <code><M>_max</M> <M>_max3</M> <M>_max4</M> <M>_max5</M></code> | every type |
| <code><M>_median</M> <M>_median4</M> <M>_median5</M></code> | every type |
| <code><M>_avg</M> <M>_avg3</M> <M>_avg4</M></code> | every type |
| <code><M>_clamp</M> <M>_saturate</M> <M>_sqr</M> <M>_cube</M></code> | every type |
| <code><M>_random</M> <M>_random_range</M></code> | every type |
| <code><M>_abs</M> <M>_sign</M> <M>_sign_zero</M></code> | <Y>i*</Y> and <Y>r*</Y> |
| <code><M>_mix</M> <M>_map</M> <M>_range</M> <M>_random_unit</M></code> | <Y>r4</Y> and <Y>r8</Y> |

### Rational Math
<Y>r4</Y> and <Y>r8</Y> also carry the full set of maths functions:
<pre>
<M>r4_sqrt</M>  <M>r4_pow</M>   <M>r4_mod</M>
<M>r4_trunc</M> <M>r4_floor</M> <M>r4_ceil</M> <M>r4_round</M>
<M>r4_sin</M>   <M>r4_cos</M>   <M>r4_tan</M>  <M>r4_sincos</M>
<M>r4_asin</M>  <M>r4_acos</M>  <M>r4_atan</M> <M>r4_atanyx</M>

<G>// And the same set for r8:</G>
<M>r8_sqrt</M><Y>,</Y> <M>r8_floor</M><Y>,</Y> <M>r8_sin</M><Y>,</Y> <LG>...</LG>
</pre>
<M>_atanyx</M> takes <LG>y</LG> then <LG>x</LG>, and gives the full-circle angle.

-------
## File I/O
Dealing with files and folders in <H>H</H> is unified across Linux and Windows.

### Path Operations
<pre>
<M>path</M><Y>(</Y> <C>"folder"</C><Y>,</Y> <C>"subfolder"</C><Y>,</Y> <C>"file.txt"</C> <Y>)</Y>
<G>// Linux: "folder/subfolder/file.txt"</G>
<G>// Windows: "folder\subfolder\file.txt"</G>

<M>program_get_path</M><Y>(</Y> <LG>OUT_BYTES</LG> <Y>)</Y>
<G>// Writes the running program's own path</G>
<G>// Outputs how many bytes were written</G>
<G>// OUT_BYTES must be path_max_size big</G>

<M>path_up_folder</M><Y>(</Y> <LG>PATH</LG> <Y>)</Y>
<G>// Removes the last part of a path</G>
<G>// "a/b/c" becomes "a/b"</G>
<G>// Edits PATH directly</G>

<M>path_get_name</M><Y>(</Y> <LG>PATH</LG> <Y>)</Y>
<G>// A ref to just the name</G>
<G>// "a/b/c.txt" gives "c.txt"</G>

<M>path_get_extension</M><Y>(</Y> <LG>PATH</LG> <Y>)</Y>
<G>// A ref to just the extension</G>
<G>// "a/b/c.txt" gives "txt"</G>

<C>path_max_size</C> <G>// Longest path (260)</G>
<C>separator</C>     <G>// "/" or "\\"</G>
</pre>

### The os_file Type
Every file operation gives you back one of these:
<pre>
<M>type</M><Y>(</Y> <LG>os_file</LG> <Y>)</Y>
<C>{</C>
	<Y>byte</Y> path<Y>[</Y> <C>path_max_size</C> <Y>]</Y><C>;</C>
	<Y>n2</Y> path_size<C>;</C>
	<Y>os_handle</Y> handle<C>;</C>
	<Y>byte const ref</Y> mapped_bytes<C>;</C>
	<Y>n8</Y> size<C>;</C>
<C>};</C>
<G>// If handle is nothing, it didn't open</G>
<G>// size is how many bytes the file holds</G>
</pre>

### File Operations
Loading:
<pre>
<Y>os_file</Y> <M>os_open_file</M><Y>(</Y> <LG>PATH</LG><Y>,</Y> <LG>OPTIONAL_PATH_SIZE</LG> <Y>)</Y><C>;</C>
<G>// Opens an existing file for reading</G>
<G>// OPTIONAL_PATH_SIZE can be used if</G>
<G>//  the path is long and pre-measured</G>

<M>os_file_ref_load</M><Y>(</Y> <LG>FILE_REF</LG><Y>,</Y> <LG>OUT_BYTES</LG> <Y>)</Y><C>;</C>
<G>// Requires OUT_BYTES to be big enough!</G>
<G>// Check FILE_REF->size first</G>

<G>// Used like:</G>
<Y>os_file</Y> f <Y>=</Y> <M>os_open_file</M><Y>(</Y> <C>"test.txt"</C> <Y>)</Y><C>;</C>
<Y>byte</Y> loaded<Y>[</Y> <M>KB</M><Y>(</Y> <C>100</C> <Y>)</Y> <Y>]</Y><C>;</C>
<M>os_file_ref_load</M><Y>(</Y> <M>ref_of</M><Y>(</Y> f <Y>),</Y> loaded <Y>)</Y><C>;</C>
<G>// This will load "test.txt",</G>
<G>//  as long as it's less than 100KB!</G>
</pre>

Saving:
<pre>
<Y>os_file</Y> <M>os_create_file</M><Y>(</Y> <LG>PATH</LG><Y>,</Y> <LG>OPTIONAL_PATH_SIZE</LG> <Y>)</Y><C>;</C>
<G>// Creates/wipes a file for writing</G>

<M>os_file_ref_save</M><Y>(</Y> <LG>FILE_REF</LG><Y>,</Y> <LG>BYTES</LG><Y>,</Y> <LG>SIZE</LG> <Y>)</Y><C>;</C>
<G>// Saves SIZE bytes from BYTES to the file</G>

<G>// Used like:</G>
<Y>os_file</Y> f <Y>=</Y> <M>os_create_file</M><Y>(</Y> <C>"output.txt"</C> <Y>)</Y><C>;</C>
<Y>byte</Y> d<Y>[]</Y> <Y>=</Y> <C>"Hello H!"</C><C>;</C>
<M>os_file_ref_save</M><Y>(</Y> <M>ref_of</M><Y>(</Y> f <Y>),</Y> d<Y>,</Y> <M>size_of_bytes</M><Y>(</Y> d <Y>) )</Y><C>;</C>
<G>// This saves "Hello H!" to output.txt</G>
</pre>

Mapping:
<pre>
<Y>os_file</Y> <M>os_map_file</M><Y>(</Y> <LG>PATH</LG><Y>,</Y> <LG>OPTIONAL_PATH_SIZE</LG> <Y>)</Y><C>;</C>
<G>// Hands the file straight to your program</G>
<G>//  as memory, with no loading step</G>
<G>// Best for large files you only read</G>

<M>os_file_ref_unmap</M><Y>(</Y> <LG>FILE_REF</LG> <Y>)</Y><C>;</C>
<G>// Gives the mapped memory back</G>

<G>// Used like:</G>
<Y>os_file</Y> f <Y>=</Y> <M>os_map_file</M><Y>(</Y> <C>"large.dat"</C> <Y>)</Y><C>;</C>
<Y>byte const ref</Y> data <Y>=</Y> f<C>.</C>mapped_bytes<C>;</C>
<G>// Now data points to the file contents</G>
<M>os_file_ref_unmap</M><Y>(</Y> <M>ref_of</M><Y>(</Y> f <Y>) )</Y><C>;</C>
</pre>

Cleanup:
<pre>
<M>os_file_ref_close</M><Y>(</Y> <LG>FILE_REF</LG> <Y>)</Y><C>;</C>
<G>// Closes an open (non-mapped) file</G>
<G>// Empties the os_file afterwards</G>

<M>os_file_ref_clear</M><Y>(</Y> <LG>FILE_REF</LG> <Y>)</Y><C>;</C>
<G>// Empties an os_file without closing</G>

<M>os_delete_file</M><Y>(</Y> <LG>PATH</LG> <Y>)</Y><C>;</C>
<G>// Deletes a file from disk</G>
<G>// Can be used like:</G>
<G>// os_delete_file( "test.txt" );</G>
<G>// or</G>
<G>// os_delete_file( f.path );</G>
</pre>

### File/Folder Utilities
<pre>
<Y>flag</Y> <M>os_file_exists</M><Y>(</Y> <LG>PATH</LG> <Y>)</Y>
<G>// yes if a file is at PATH</G>

<Y>flag</Y> <M>os_folder_exists</M><Y>(</Y> <LG>PATH</LG> <Y>)</Y>
<G>// yes if a folder is at PATH</G>

<M>os_create_folder</M><Y>(</Y> <LG>PATH</LG> <Y>)</Y>
<G>// Create a new folder at PATH</G>

<M>os_delete_folder</M><Y>(</Y> <LG>PATH</LG> <Y>)</Y>
<G>// Deletes a folder and everything in it</G>
<G>// Does nothing if the folder isn't there</G>
</pre>

### Folder Contents
<pre>
<Y>n2</Y> <M>os_get_entries</M><Y>(</Y> <LG>PATH</LG><Y>,</Y> <LG>ENTRIES</LG><Y>,</Y> <LG>MAX</LG><Y>,</Y> <LG>TYPE</LG><Y>,</Y> <LG>SEPARATOR</LG> <Y>)</Y><C>;</C>
<G>// Fills ENTRIES with names, outputs how many</G>
<G>// ENTRIES must be byte[][ path_max_size ]</G>
<G>// SEPARATOR puts a "/" after folder names</G>

<Y>n2</Y> <M>os_get_files</M><Y>(</Y> <LG>PATH</LG><Y>,</Y> <LG>ENTRIES</LG><Y>,</Y> <LG>MAX</LG> <Y>)</Y><C>;</C>
<G>// Only files</G>

<Y>n2</Y> <M>os_get_folders</M><Y>(</Y> <LG>PATH</LG><Y>,</Y> <LG>ENTRIES</LG><Y>,</Y> <LG>MAX</LG><Y>,</Y> <LG>OPTIONAL_SEPARATOR</LG> <Y>)</Y><C>;</C>
<G>// Only folders, with separators by default</G>

<G>// Entry types:</G>
<C>entry_files</C>   <G>// Files only</G>
<C>entry_folders</C> <G>// Folders only</G>
<C>entry_any</C>     <G>// Both files and folders</G>

<G>// Used like:</G>
<Y>byte</Y> entries<Y>[</Y> <C>100</C> <Y>][</Y> <C>path_max_size</C> <Y>]</Y><C>;</C>
<Y>n2</Y> count <Y>=</Y> <M>os_get_files</M><Y>(</Y> <C>"."</C><Y>,</Y> entries<Y>,</Y> <C>100</C> <Y>)</Y><C>;</C>
<M>iter</M><Y>(</Y> i<Y>,</Y> count <Y>)</Y>
<C>{</C>
	<M>print</M><Y>(</Y> entries<Y>[</Y>i<Y>]</Y> <Y>)</Y><C>;</C>
	<M>print_newline</M><Y>()</Y><C>;</C>
<C>}</C>
</pre>

-------
## Terminal

### Printing
<pre>
<M>print</M><Y>(</Y> <C>"Hello, World!"</C> <Y>)</Y>
<G>// Buffers null-terminated bytes</G>
<G>// It will only display once it hits a '\n',</G>
<G>//  or when print_show() is called</G>

<M>print_count</M><Y>(</Y> <LG>BYTES</LG><Y>,</Y> <C>100</C> <Y>)</Y>
<G>// Output exactly 100 bytes</G>
<G>// Doesn't need a '\0'</G>

<M>print_newline</M><Y>()</Y>  <G>// Output a "\n"</G>
<M>print_tab</M><Y>()</Y>      <G>// Output a "\t"</G>
<M>print_separator</M><Y>()</Y> <G>// Output a "/" or "\\"</G>

<M>print_show</M><Y>()</Y>
<G>// Display everything buffered right now</G>

<M>print_clear</M><Y>()</Y>
<G>// Clear the whole terminal screen</G>
</pre>

### Terminal Input
<pre>
<M>command_get_input</M><Y>(</Y> <LG>BYTES</LG><Y>,</Y> <LG>SIZE</LG> <Y>)</Y>
<G>// Waits for a line typed by the user</G>
<G>// Writes it into BYTES, without the '\n'</G>

<G>// Used like:</G>
<Y>byte</Y> answer<Y>[</Y> <C>64</C> <Y>]</Y><C>;</C>
<M>print</M><Y>(</Y> <C>"name? "</C> <Y>)</Y><C>;</C>
<M>print_show</M><Y>()</Y><C>;</C>
<M>command_get_input</M><Y>(</Y> answer<Y>,</Y> <C>64</C> <Y>)</Y><C>;</C>
</pre>

### Running Commands
<pre>
<M>command</M><Y>(</Y> <LG>COMMAND</LG> <Y>)</Y>
<G>// Runs COMMAND, shows all of its output</G>

<M>command_silent</M><Y>(</Y> <LG>COMMAND</LG><Y>,</Y> <LG>OPTIONAL_DETACH</LG> <Y>)</Y>
<G>// Runs COMMAND, hides all of its output</G>
<G>// OPTIONAL_DETACH is no by default:</G>
<G>//  yes leaves it running and moves on</G>
<G>// Outputs command_failed if it can't run</G>

<M>command_read_open</M><Y>(</Y> <LG>COMMAND</LG> <Y>)</Y>
<M>command_read_open_silent</M><Y>(</Y> <LG>COMMAND</LG> <Y>)</Y>
<G>// Runs COMMAND and hands you its output</G>
<G>//  as an os_handle you can read lines from</G>

<M>command_read_close</M><Y>(</Y> <LG>OS_HANDLE</LG> <Y>)</Y>
<M>command_read_close_silent</M><Y>(</Y> <LG>OS_HANDLE</LG> <Y>)</Y>
<G>// Closes what the matching open gave you</G>

<C>command_max_size</C> <G>// Longest command (1024)</G>
<C>command_failed</C>   <G>// -1</G>
</pre>

### Waiting
<pre>
<M>sleep</M><Y>(</Y> <LG>MILLISECONDS</LG> <Y>)</Y>
<G>// Pauses the program</G>
<G>// sleep( 1000 ) waits one second</G>
</pre>

-------
## Platform Abstraction

<H>H</H> automatically detects and adapts to the platform:
<pre>
<C>OS_LINUX</C>   <G>// 1 if Linux, 0 if not</G>
<C>OS_WINDOWS</C> <G>// 1 if Windows, 0 if not</G>
<C>OS_MACOS</C>   <G>// 1 if macOS, 0 if not</G>
<C>OS_UNKNOWN</C> <G>// 1 if none of the above</G>
<C>OS_NAME</C>    <G>// Bytes: "Linux", "Windows", ...</G>

<C>COMPILER_GCC</C>  <G>// 1 if built with GCC</G>
<C>COMPILER_TCC</C>  <G>// 1 if built with TCC</G>
<C>COMPILER_NAME</C> <G>// Bytes: "GCC", "TCC", ...</G>
</pre>

To write both versions of something at once, and have only one of them survive:
<pre>
<M>OS_PICK</M><Y>(</Y> <LG>LINUX_CODE</LG><Y>,</Y> <LG>WINDOWS_CODE</LG> <Y>)</Y>
<G>// The unused one is thrown away</G>
<G>//  before the compiler ever sees it</G>
<G>// So it can name things that don't</G>
<G>//  exist on the other platform</G>

<M>PICK</M><Y>(</Y> <LG>1_OR_0</LG><Y>,</Y> <LG>THIS</LG><Y>,</Y> <LG>OR_THIS</LG> <Y>)</Y>
<G>// The same, for any compile-time 1 or 0</G>
</pre>
<DG>PICK chooses text before compiling, pick chooses a value while running</DG>

-------
## Utility Macros

### Default Inputs
<pre>
<M>DEFAULT</M><Y>(</Y> <LG>VAL</LG><Y>,</Y> <LG>INPUT</LG> <Y>)</Y>
<G>// Becomes INPUT if given, otherwise VAL</G>
<G>// This is how OPTIONAL_ inputs work</G>

<M>DEFAULTS</M><Y>( (</Y> <LG>TUPLE</LG> <Y>),</Y> <LG>INPUTS...</LG> <Y>)</Y>
<G>// Used like:</G>
<C>#define</C> <M>foo</M><Y>(</Y> <LG>NAME</LG><Y>,</Y> <LG>INPUTS...</LG> <Y>)</Y><G>\</G>
	<LG>NAME</LG><Y>(</Y> <M>DEFAULTS</M><Y>( (</Y> <C>1</C><Y>,</Y> <C>2</C><Y>,</Y> <C>3</C> <Y>),</Y> <LG>INPUTS</LG> <Y>) )</Y>
<G>// foo( test ) -> test( 1, 2, 3 )</G>
<G>// foo( test, 7 ) -> test( 7, 2, 3 )</G>
<G>// foo( test, 7, 8 ) -> test( 7, 8, 3 )</G>
<G>// foo( test, 7, 8, 9 ) -> test( 7, 8, 9 )</G>
<G>// foo( test, 7,, 9 ) -> test( 7,, 9 )</G>
</pre>

### Compile-Time Helpers
<pre>
<M>COUNT_INPUTS</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>C</LG> <Y>)</Y>
<G>// Becomes 3, before compiling</G>
<G>// Up to 16 inputs</G>

<M>AS_BYTES</M><Y>(</Y> <LG>ANYTHING</LG> <Y>)</Y>
<G>// AS_BYTES( hello ) converts to "hello"</G>
<G>// AS_BYTES( ":)" ) converts to "\":)\""</G>

<M>CHAIN</M><Y>(</Y> <LG>LEFT</LG><Y>,</Y> <LG>RIGHT</LG><Y>,</Y> <LG>MIDDLE</LG><Y>,</Y> <LG>INPUTS...</LG> <Y>)</Y>
<G>// LEFT a RIGHT MIDDLE LEFT b RIGHT ...</G>
<G>// Can be used like:</G>
<G>// #define F( INPUTS... ) CHAIN( ,, +, INPUTS )</G>
<G>// F( A, B, C ) converts to A + B + C</G>

<M>CHAIN_PAREN</M><Y>(</Y> <LG>LEFT</LG><Y>,</Y> <LG>RIGHT</LG><Y>,</Y> <LG>MIDDLE</LG><Y>,</Y> <LG>INPUTS...</LG> <Y>)</Y>
<G>// The same, but wraps each one in ( )</G>
<G>// This is how any() and all() are made</G>

<M>COMMA_IF_INPUTS</M><Y>(</Y> <LG>INPUTS...</LG> <Y>)</Y>
<G>// Inserts a comma only if INPUTS exist</G>

<M>PASTE_IF_INPUTS</M><Y>(</Y> <LG>CODE</LG><Y>,</Y> <LG>INPUTS...</LG> <Y>)</Y>
<G>// Pastes CODE only if INPUTS exist</G>

<M>EVAL</M><Y>(</Y> <LG>INPUTS...</LG> <Y>)</Y>
<G>// Expands INPUTS one more time</G>

<M>JOIN</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG> <Y>)</Y>
<G>// Sticks two names into one</G>
<G>// JOIN( entity_, player ) is entity_player</G>

<M>START_DEF</M> <LG>...</LG> <M>END_DEF</M>
<G>// Wraps a multi-statement #define so it</G>
<G>//  behaves like one statement</G>

<M>REQUIRE_SEMICOLON</M>
<G>// Forces a semicolon after a macro use</G>
</pre>

### Memory Size Constants
<pre>
<M>KB</M><Y>(</Y> <LG>N</LG> <Y>)</Y> <G>// N * 1,000</G>
<M>MB</M><Y>(</Y> <LG>N</LG> <Y>)</Y> <G>// N * 1,000,000</G>
<M>GB</M><Y>(</Y> <LG>N</LG> <Y>)</Y> <G>// N * 1,000,000,000</G>
<M>KiB</M><Y>(</Y> <LG>N</LG> <Y>)</Y> <G>// N * 1,024</G>
<M>MiB</M><Y>(</Y> <LG>N</LG> <Y>)</Y> <G>// N * 1,048,576</G>
<M>GiB</M><Y>(</Y> <LG>N</LG> <Y>)</Y> <G>// N * 1,073,741,824</G>
</pre>

### Many Inputs
For functions that take an unknown number of inputs:
<pre>
<Y>inputs_list</Y> <LG>NAME</LG><C>;</C>
<G>// Holds the extra inputs</G>

<M>inputs_init</M><Y>(</Y> <LG>LIST</LG><Y>,</Y> <LG>LAST_NAMED_INPUT</LG> <Y>)</Y>
<G>// Start reading after LAST_NAMED_INPUT</G>

<M>inputs_next</M><Y>(</Y> <LG>LIST</LG><Y>,</Y> <LG>TYPE</LG> <Y>)</Y>
<G>// Read the next one, as TYPE</G>

<M>inputs_copy</M><Y>(</Y> <LG>FROM</LG><Y>,</Y> <LG>TO</LG> <Y>)</Y>
<G>// Copy a list, to read it twice</G>

<M>inputs_end</M><Y>(</Y> <LG>LIST</LG> <Y>)</Y>
<G>// Finish reading</G>

<G>// Used like:</G>
<Y>i4</Y> <M>add_all</M><Y>( n1 const</Y> count<Y>,</Y> <LG>...</LG> <Y>)</Y>
<C>{</C>
	<Y>inputs_list</Y> inputs<C>;</C>
	<M>inputs_init</M><Y>(</Y> inputs<Y>,</Y> count <Y>)</Y><C>;</C>
	
	<Y>i4</Y> total <Y>=</Y> <C>0;</C>
	<M>iter</M><Y>(</Y> i<Y>,</Y> count <Y>)</Y>
	<C>{</C>
		total <Y>+=</Y> <M>inputs_next</M><Y>(</Y> inputs<Y>, i4</Y> <Y>)</Y><C>;</C>
	<C>}</C>
	
	<M>inputs_end</M><Y>(</Y> inputs <Y>)</Y><C>;</C>
	<M>out</M> total<C>;</C>
<C>}</C>
</pre>

-------
## Starting a Program
Every <H>H</H> program begins at <M>start</M>:
<pre>
<M>start</M>
<C>{</C>
	<M>out</M> <C>success;</C>
<C>}</C>
</pre>

Whatever was typed after the program's name is waiting there for you:
<pre>
<C>start_inputs_count</C>
<G>// How many inputs there are</G>
<G>// Always at least 1</G>

<C>start_inputs</C>
<G>// start_inputs[ 0 ] is the program itself</G>
<G>// start_inputs[ 1 ] is the first input</G>
<G>// ...and so on</G>
</pre>

-------
## Example Programs

### Hello World
<pre>
<Y>#include</Y> <C>"H.h"</C>

<M>start</M>
<C>{</C>
	<M>print</M><Y>(</Y> <C>"Hello, World!"</C> <Y>)</Y><C>;</C>
	<M>print_newline</M><Y>()</Y><C>;</C>
	<M>out</M> <C>success;</C>
<C>}</C>
</pre>

### File Processing
Reads the file you name, strips the spacing out of it, turns every <C>e</C> into a <C>3</C>, and prints the result.
<pre>
<Y>#include</Y> <C>"H.h"</C>

<M>start</M>
<C>{</C>
	<M>out_if</M><Y>(</Y> <C>start_inputs_count</C> <Y><</Y> <C>2</C> <Y>)</Y> <C>failure;</C>
	
	<Y>os_file</Y> file <Y>=</Y> <M>os_open_file</M><Y>(</Y> <C>start_inputs</C><Y>[</Y> <C>1</C> <Y>]</Y> <Y>)</Y><C>;</C>
	<M>if_nothing</M><Y>(</Y> file<C>.</C>handle <Y>)</Y>
	<C>{</C>
		<M>print</M><Y>(</Y> <C>"Failed to open file"</C> <Y>)</Y><C>;</C>
		<M>print_newline</M><Y>()</Y><C>;</C>
		<M>out</M> <C>failure;</C>
	<C>}</C>
	
	<Y>byte</Y> input<Y>[</Y> <M>KB</M><Y>(</Y> <C>100</C> <Y>)</Y> <Y>]</Y><C>;</C>
	<Y>byte</Y> output<Y>[</Y> <M>KB</M><Y>(</Y> <C>100</C> <Y>)</Y> <Y>]</Y><C>;</C>
	
	<M>if</M><Y>(</Y> file<C>.</C>size <Y>></Y> <M>size_of</M><Y>(</Y> input <Y>)</Y> <Y>)</Y>
	<C>{</C>
		<M>os_file_ref_close</M><Y>(</Y> <M>ref_of</M><Y>(</Y> file <Y>) )</Y><C>;</C>
		<M>out</M> <C>failure;</C>
	<C>}</C>
	
	<M>os_file_ref_load</M><Y>(</Y> <M>ref_of</M><Y>(</Y> file <Y>),</Y> input <Y>)</Y><C>;</C>
	
	<Y>byte ref</Y> output_ref <Y>=</Y> output<C>;</C>
	<M>iter</M><Y>(</Y> byte_index<Y>,</Y> file<C>.</C>size <Y>)</Y>
	<C>{</C>
		<Y>byte const</Y> value <Y>=</Y> input<Y>[</Y> byte_index <Y>]</Y><C>;</C>
		<M>with</M><Y>(</Y> value <Y>)</Y>
		<C>{</C>
			<M>when</M><Y>(</Y> <C>' '</C><Y>,</Y> <C>'\t'</C><Y>,</Y> <C>'\n'</C><Y>,</Y> <C>'\r'</C> <Y>)</Y> <M>skip</M><C>;</C>
			
			<M>when</M><Y>(</Y> <C>'E'</C><Y>,</Y> <C>'e'</C> <Y>)</Y> <M>bytes_set_move</M><Y>(</Y> output_ref<Y>,</Y> <C>'3'</C> <Y>)</Y><C>;</C>
			
			<M>other</M> <M>bytes_set_move</M><Y>(</Y> output_ref<Y>,</Y> value <Y>)</Y><C>;</C>
		<C>}</C>
	<C>}</C>
	<M>bytes_end</M><Y>(</Y> output_ref <Y>)</Y><C>;</C>
	
	<M>os_file_ref_close</M><Y>(</Y> <M>ref_of</M><Y>(</Y> file <Y>) )</Y><C>;</C>
	
	<M>print</M><Y>(</Y> output <Y>)</Y><C>;</C>
	<M>print_newline</M><Y>()</Y><C>;</C>
	<M>out</M> <C>success;</C>
<C>}</C>
</pre>

### Folder Report
Counts what's in a folder, and writes the list to a file.
<pre>
<Y>#include</Y> <C>"H.h"</C>

<M>start</M>
<C>{</C>
	<Y>byte const ref const</Y> folder <Y>=</Y> <M>pick</M><Y>(</Y> <C>start_inputs_count</C> <Y>></Y> <C>1</C><Y>,</Y> <C>start_inputs</C><Y>[</Y> <C>1</C> <Y>],</Y> <C>"."</C> <Y>)</Y><C>;</C>
	
	<Y>byte</Y> entries<Y>[</Y> <C>256</C> <Y>][</Y> <C>path_max_size</C> <Y>]</Y><C>;</C>
	<Y>n2 const</Y> count <Y>=</Y> <M>os_get_files</M><Y>(</Y> folder<Y>,</Y> entries<Y>,</Y> <C>256</C> <Y>)</Y><C>;</C>
	
	<Y>byte</Y> report<Y>[</Y> <M>KB</M><Y>(</Y> <C>64</C> <Y>)</Y> <Y>]</Y><C>;</C>
	<Y>byte ref</Y> report_ref <Y>=</Y> report<C>;</C>
	
	<M>n2_to_bytes_move</M><Y>(</Y> count<Y>,</Y> report_ref <Y>)</Y><C>;</C>
	<M>bytes_paste_move</M><Y>(</Y> report_ref<Y>,</Y> <C>" files"</C> <Y>)</Y><C>;</C>
	<M>bytes_newline_move</M><Y>(</Y> report_ref <Y>)</Y><C>;</C>
	
	<M>iter</M><Y>(</Y> entry_index<Y>,</Y> count <Y>)</Y>
	<C>{</C>
		<M>bytes_paste_move</M><Y>(</Y> report_ref<Y>,</Y> entries<Y>[</Y> entry_index <Y>]</Y> <Y>)</Y><C>;</C>
		<M>bytes_newline_move</M><Y>(</Y> report_ref <Y>)</Y><C>;</C>
	<C>}</C>
	<M>bytes_end</M><Y>(</Y> report_ref <Y>)</Y><C>;</C>
	
	<Y>os_file</Y> out_file <Y>=</Y> <M>os_create_file</M><Y>(</Y> <C>"report.txt"</C> <Y>)</Y><C>;</C>
	<M>os_file_ref_save</M><Y>(</Y> <M>ref_of</M><Y>(</Y> out_file <Y>),</Y> report<Y>,</Y> <M>bytes_measure</M><Y>(</Y> report <Y>) )</Y><C>;</C>
	<M>os_file_ref_close</M><Y>(</Y> <M>ref_of</M><Y>(</Y> out_file <Y>) )</Y><C>;</C>
	
	<M>print</M><Y>(</Y> report <Y>)</Y><C>;</C>
	<M>out</M> <C>success;</C>
<C>}</C>
</pre>

-------
## License

H is released under CC0 (Creative Commons Zero) - effectively public domain. FOSS forever.
 
-------
