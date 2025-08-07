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
If the reference will be changed:
<pre>
<LG>TYPE</LG> <Y>ref</Y> <LG>NAME</LG>
<G>// A reference to a TYPE</G>
<G>// Can be changed</G>
</pre>

Otherwise, make it constant:
<pre>
<LG>TYPE</LG> <Y>const_ref</Y> <LG>NAME</LG>
<G>// A constant reference to a TYPE</G>
<G>// Cannot be changed!</G>
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

<Y>temp</Y> <LG>TYPE VAR</LG> <G>// VAR is temporary</G>
<G>// Faster read/write time than without temp
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
With ref/const_ref too:
<pre>
<Y>perm</Y><LG>/</LG><Y>temp const</Y> <LG>TYPE</LG> <Y>ref</Y> <LG>NAME</LG>
<G>// A permanent-or-temporary</G>
<G>//  constant TYPE reference</G>

<Y>perm</Y><LG>/</LG><Y>temp const</Y> <LG>TYPE</LG> <Y>const_ref</Y> <LG>NAME</LG>
<G>// A permanent-or-temporary</G>
<G>//  constant TYPE constant-reference</G>
</pre>

### Mutation Permissions
<pre>
<Y>const</Y> <LG>TYPE</LG> <Y>const_ref</Y> <LG>NAME</LG>
<G>// NAME and val_of NAME cannot change</G>

<LG>TYPE</LG> <Y>const_ref</Y> <LG>NAME</LG>
<G>// NAME cannot change, but val_of NAME can</G>

<Y>const</Y> <LG>TYPE</LG> <Y>ref</Y> <LG>NAME</LG>
<G>// NAME can change, but val_of NAME cannot</G>

<LG>TYPE</LG> <Y>ref</Y> <LG>NAME</LG>
<G>// NAME and val_of NAME can change</G>
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

### Other Types
<pre>
<Y>flag</Y> <LG>VAR</LG>
<G>// A flag can either be:</G>
<C>yes</C> <G>// 1</G>
<G>// or</G>
<C>no</C>  <G>// 0</G>

<M>flip</M><Y>(</Y> <LG>FLAG</LG> <Y>)</Y> <G>// no to yes, yes to no</G>
<G>// flag is_ready = no;</G>
<G>// flip( is_ready );</G>
<G>// is_ready is now yes</G>

<M>pick</M><Y>(</Y> <LG>FLAG</LG><Y>,</Y> <LG>A</LG><Y>,</Y> <LG>B</LG> <Y>)</Y> <G>// Ternary operator</G>
<G>// If FLAG is yes, pick A, else pick B</G>
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
	<M>DEF_START</M><G>\</G>
	<C>{</C><G>\</G>
		<M>type_of</M><Y>(</Y> A <Y>)</Y> _temp <Y>=</Y> A<C>;</C><G>\</G>
		A <Y>=</Y> B<C>;</C><G>\</G>
		B <Y>=</Y> _temp<C>;</C><G>\</G>
	<C>}</C><G>\</G>
	<M>DEF_END</M>
<G>// Which automatically deduces the type</G>
<G>//  for swapping any 2 same-type variables</G>
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
	<Y>const</Y> <LG>TYPE NAME</LG><C>;</C>
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

If there's going to be less than 256 elements, the default group-type is a <H>byte</H>:
<pre>
<M>group</M><Y>(</Y> <LG>NAME</LG> <Y>)</Y>
<C>{</C>
	<LG>...</LG>
<C>};</C>
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
	<C>}</C>
	<Y>entity_type</Y> t <Y>=</Y> entity_enemy<C>;</C>
<C>}</C>
<G>// entity_type and the elements are</G>
<G>//  not accessible outside the scope</G>
</pre>

-------
## Execution Control
The program is "read" by the computer in a downwards flow, as if it's reading a book.
To move where it's "reading", you can use:
<H>skip</H> <LG>(skips the rest of the scope)</LG>,
<H>jump</H> <LG>(jumps to a POINT)</LG>,
or <H>next</H> <LG>(next scope-iteration)</LG>

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

There's also <LG>*</LG><M>_if</M> forms of these that can often be easier to read:
<pre>
<M>skip_if</M><Y>(</Y> <LG>FLAG</LG> <Y>)</Y><C>;</C>
<G>// Skip if FLAG is yes</G>

<M>jump_if</M><Y>(</Y> <LG>FLAG</LG> <Y>)</Y> <LG>POINT</LG><C>;</C>
<G>// Jump to POINT if FLAG is yes</G>

<M>next_if</M><Y>(</Y> <LG>FLAG</LG> <Y>)</Y><C>;</C>
<G>// Jump to next iteration if FLAG is yes</G>

<M>out_if</M><Y>(</Y> <LG>FLAG</LG> <Y>)</Y> <LG>VAL</LG><C>;</C>
<G>// Output VAL if FLAG is yes</G>
<G>// Can be empty if fn doesn't output:</G>
<G>// out_if( FLAG );</G>
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
		<G>// Code only if val is 1, 2, or 3</G>
		<M>skip</M><C>;</C> <G>// Skip the rest</G>
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
	<G>// If any of the arguments are yes</G>
<C>}</C>

<M>if_all</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>C</LG><Y>,</Y> <LG>...</LG> <Y>)</Y>
<C>{</C>
	<G>// If all of the arguments are yes</G>
<C>}</C>

<M>if_none</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>C</LG><Y>,</Y> <LG>...</LG> <Y>)</Y>
<C>{</C>
	<G>// If none of the arguments are yes</G>
<C>}</C>

<M>if_not_all</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>C</LG><Y>,</Y> <LG>...</LG> <Y>)</Y>
<C>{</C>
	<G>// If not all of the arguments are yes</G>
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

<M>iter_grid</M><Y>(</Y> <LG>X</LG><Y>,</Y> <LG>Y</LG><Y>,</Y> <LG>WIDTH</LG><Y>,</Y> <LG>HEIGHT</LG> <Y>)</Y>
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
<C>}</C> <G>// The program will jump back when done</G></G>
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
<M>fn</M> <M>greet</M><Y>( byte ref</Y> name <Y>)</Y>
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
</pre>

### Function References
<pre>
<M>fn_ref</M><Y>(</Y> <LG>OUTPUT</LG><Y>,</Y> <LG>NAME</LG><Y>,</Y> <LG>ARG_TYPES...</LG> <Y>)</Y>
<G>// Declare a function reference</G>

<M>fn_type</M><Y>(</Y> <LG>OUTPUT</LG><Y>,</Y> <LG>ARG_TYPES...</LG> <Y>)</Y>
<G>// Create a function type</G>
</pre>

-------
## Objects
An object is a reference to a type, that's often created to exist outside of scopes.
<pre>
<M>object</M><Y>(</Y> <LG>NAME</LG> <Y>)</Y>
<C>{</C>
	<LG>TYPE NAME_A</LG><C>;</C>
	<LG>TYPE NAME_B</LG><C>;</C>
	<LG>...</LG>
<C>};</C> <DG><- the semicolon is required!</DG>

<M>object_fn</M><Y>(</Y> <LG>NAME</LG><Y>,</Y> <LG>FN</LG><Y>,</Y> <LG>OPTIONAL_ARGS...</LG> <Y>)</Y>
<C>{</C>
	<G>// "this" is available to access elements</G>
	<G>//  from within the object:</G>
	<LG>this</LG><C>-></C><LG>NAME_A</LG> <Y>=</Y> <LG>...</LG><C>;</C>
<C>}</C>
<G>// The object-function made is:</G>
<G>//  NAME_FN( NAME this, OPTIONAL_ARGS... )</G>
</pre>

Usage example:
<pre>
<M>object</M><Y>(</Y> player <Y>)</Y>
<C>{</C>
	<Y>i2</Y> hp<C>;</C>
	<Y>r8</Y> x<C>;</C>
	<Y>r8</Y> y<C>;</C>
<C>};</C>
<Y>global player</Y> main_player <Y>=</Y> <C>nothing;</C>
<G>// It needs to be nothing, then made after</G>

<M>object_fn</M><Y>( player,</Y> <M>move</M><Y>,</Y> x<Y>,</Y> y <Y>)</Y>
<C>{</C>
	this<C>-></C>x <Y>+=</Y> x<C>;</C>
	this<C>-></C>y <Y>+=</Y> y<C>;</C>
<C>}</C>

<C>{</C> <G>// In a scope somewhere</G>
	main_player <Y>=</Y> <M>new_object</M><Y>( player )</Y><C>;</C>
	main_player<C>-></C>hp <Y>=</Y> <C>100;</C>
	main_player<C>-></C>x <Y>=</Y> <C>-20.5;</C>
	main_player<C>-></C>y <Y>=</Y> <C>77.0;</C>
	<M>player_move</M><Y>(</Y> main_player<C>, 1.25, -5.1</C> <Y>)</Y><C>;</C>
	<G>// main_player->x is now -19.25, and</G>
	<G>// main_player->y is now 71.9</G>
<C>}</C>
</pre>

-------
## Memory Operations

### Dynamic Memory Allocation
<pre>
<M>new_ref</M><Y>(</Y> <LG>TYPE</LG><Y>,</Y> <LG>OPTIONAL_SIZE</LG> <Y>)</Y>
<G>// Allocate memory for TYPE (default 1)</G>
<G>// OPTIONAL_SIZE is 1 by default</G>
<G>// Memory is zero-initialized</G>

<M>new_bytes</M><Y>(</Y> <LG>OPTIONAL_SIZE</LG> <Y>)</Y>
<G>// Allocate raw bytes</G>
<G>// OPTIONAL_SIZE is 1 by default</G>
<G>// Memory is not initialized</G>
</pre>

### Byte Reference Operations
<pre>
<M>bytes_copy</M><Y>(</Y> <LG>FROM</LG><Y>,</Y> <LG>SIZE</LG><Y>,</Y> <LG>TO</LG> <Y>)</Y>
<G>// Copy SIZE bytes from FROM to TO</G>

<M>bytes_copy_until</M><Y>(</Y> <LG>FROM</LG><Y>,</Y> <LG>TO</LG><Y>,</Y> <LG>CHAR</LG><Y>,</Y> <LG>MAX</LG> <Y>)</Y>
<G>// Copy until CHAR is found or MAX bytes</G>

<M>bytes_copy_until_any</M><Y>(</Y> <LG>FROM</LG><Y>,</Y> <LG>TO</LG><Y>,</Y> <LG>DELIMS</LG> <Y>)</Y>
<G>// Copy until any delimiter in DELIMS</G>

<M>bytes_copy_internal</M><Y>(</Y> <LG>FROM</LG><Y>,</Y> <LG>SIZE</LG><Y>,</Y> <LG>TO</LG> <Y>)</Y>
<G>// Copy SIZE bytes, handles overlapping</G>

<M>bytes_fill</M><Y>(</Y> <LG>PTR</LG><Y>,</Y> <LG>VALUE</LG><Y>,</Y> <LG>SIZE</LG> <Y>)</Y>
<G>// Fill SIZE bytes with VALUE</G>

<M>bytes_clear</M><Y>(</Y> <LG>PTR</LG><Y>,</Y> <LG>SIZE</LG> <Y>)</Y>
<G>// Clear SIZE bytes to zero</G>

<M>bytes_compare</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>SIZE</LG> <Y>)</Y>
<G>// Compare SIZE bytes</G>
<G>// Outputs 0 if equal,</G>
<G>//  negative if A < B, positive if A > B</G>
</pre>

### Null-Terminated Operations
<pre>
<M>bytes_measure</M><Y>(</Y> <LG>STR</LG> <Y>)</Y>
<G>// Count bytes until '\0'</G>

<M>bytes_paste</M><Y>(</Y> <LG>FROM</LG><Y>,</Y> <LG>TO</LG> <Y>)</Y>
<G>// Copy bytes until '\0'</G>

<M>bytes_end</M><Y>(</Y> <LG>REF</LG> <Y>)</Y>
<G>// Set REF byte to '\0'</G>
<G>// Doesn't measure REF!</G>
</pre>

### Dynamic Copy Operations
<pre>
<M>assign_move</M><Y>(</Y> <LG>FROM_REF</LG><Y>,</Y> <LG>TO_REF</LG> <Y>)</Y>
<G>// Copy value and increment both refs</G>

<M>bytes_copy_move</M><Y>(</Y> <LG>FROM</LG><Y>,</Y> <LG>SIZE</LG><Y>,</Y> <LG>TO</LG> <Y>)</Y>
<G>// Copy SIZE bytes then</G>
<G>//  moves the TO ref by SIZE</G>

<M>bytes_paste_move</M><Y>(</Y> <LG>FROM</LG><Y>,</Y> <LG>TO</LG> <Y>)</Y>
<G>// Pastes null-terminated FROM to TO</G>
<G>//  moves the TO ref by FROM size</G>

<M>bytes_set_move</M><Y>(</Y> <LG>BYTE</LG><Y>,</Y> <LG>TO</LG> <Y>)</Y>
<G>// Set single byte and move the TO ref by 1</G>
</pre>

-------
## Math Operations

<pre>
<M>MIN</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG> <Y>)</Y>
<G>// Smaller of A or B</G>

<M>MAX</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG> <Y>)</Y>
<G>// Larger of A or B</G>

<M>MIN3</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>C</LG> <Y>)</Y>
<G>// Smallest of three</G>

<M>MIN4</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>C</LG><Y>,</Y> <LG>D</LG> <Y>)</Y>
<G>// Smallest of four</G>

<M>MAX3</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>C</LG> <Y>)</Y>
<G>// Largest of three</G>

<M>MAX4</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>C</LG><Y>,</Y> <LG>D</LG> <Y>)</Y>
<G>// Largest of four</G>

<M>MEAN</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG> <Y>)</Y>
<G>// Average of A and B</G>

<M>CLAMP</M><Y>(</Y> <LG>VAL</LG><Y>,</Y> <LG>MIN</LG><Y>,</Y> <LG>MAX</LG> <Y>)</Y>
<G>// Keep VAL between MIN and MAX</G>

<M>ABS</M><Y>(</Y> <LG>VAL</LG> <Y>)</Y>
<G>// Remove negative sign</G>

<M>SIGN</M><Y>(</Y> <LG>VAL</LG> <Y>)</Y>
<G>// 1 if positive or 0, -1 if negative</G>

<M>SIGN_ZERO</M><Y>(</Y> <LG>VAL</LG> <Y>)</Y>
<G>// 0 if 0, 1 if positive, -1 if negative</G>

<M>SQR</M><Y>(</Y> <LG>VAL</LG> <Y>)</Y>
<G>// VAL * VAL</G>

<M>CUBE</M><Y>(</Y> <LG>VAL</LG> <Y>)</Y>
<G>// VAL * VAL * VAL</G>

<M>SNAP</M><Y>(</Y> <LG>VAL</LG><Y>,</Y> <LG>MULTIPLE</LG> <Y>)</Y>
<G>// Round VAL to nearest MULTIPLE</G>
</pre>

-------
## File I/O
Dealing with files and folders in <H>H</H> is unified across Linux and Windows.

### Path Operations
<pre>
<M>path</M><Y>(</Y> <C>"folder"</C><Y>,</Y> <C>"subfolder"</C><Y>,</Y> <C>"file.txt"</C> <Y>)</Y>
<G>// Linux: "folder/subfolder/file.txt"</G>
<G>// Windows: "folder\subfolder\file.txt"</G>

<M>path_up_folder</M><Y>(</Y> <LG>PATH_BUFFER</LG> <Y>)</Y>
<G>// Remove last folder from path</G>
<G>// "a/b/c" becomes "a/b"</G>

<Y>byte ref</Y> <LG>exe_path</LG> <Y>=</Y> <M>get_exe_path</M><Y>()</Y>;
<G>// Get full path to current executable</G>

<C>PATH_MAX_SIZE</C> <G>// Maximum path length (260)</G>

<C>SEPARATOR</C> <G>// "/" on Unix, "\\" on Windows</G>
</pre>

### File Operations
Loading:
<pre>
<Y>file</Y> <M>open_file</M><Y>(</Y> <LG>PATH</LG><Y>,</Y> <LG>OPTIONAL_PATH_SIZE</LG> <Y>)</Y><C>;</C>
<G>// Opens a file,</G>
<G>//  outputs a file-type for loading</G>
<G>// OPTIONAL_PATH_SIZE can be used if</G>
<G>//  the path is long and pre-measured</G>

<M>file_load</M><Y>(</Y> <LG>FILE_REF</LG><Y>,</Y> <LG>OUTPUT</LG> <Y>)</Y><C>;</C>
<G>// Requires OUTPUT to be a byte-reference,</G>
<G>//  which must be big enough!</G>

<G>// Used like:</G>
<Y>file</Y> f <Y>=</Y> <M>open_file</M><Y>(</Y> <C>"test.txt"</C> <Y>)</Y><C>;</C>
<Y>byte</Y> loaded<M>[ KB</M><Y>(</Y> <C>100</C> <Y>)</Y> <M>]</M><C>;</C>
<M>file_load</M><Y>(</Y> <M>ref_of</M><Y>(</Y> f <Y>),</Y> loaded <Y>)</Y><C>;</C>
<G>// This will load "test.txt",</G>
<G>//  as long as it's less than 100KB!</G>
</pre>

Saving:
<pre>
<Y>file</Y> <M>new_file</M><Y>(</Y> <LG>PATH</LG><Y>,</Y> <LG>OPTIONAL_PATH_SIZE</LG> <Y>)</Y><C>;</C>
<G>// Creates/wipes a file,</G>
<G>//  outputs a file-type for saving</G>
<G>// OPTIONAL_PATH_SIZE can be used if</G>
<G>//  the path is long and pre-measured</G>

<M>file_save</M><Y>(</Y> <LG>FILE_REF</LG><Y>,</Y> <LG>DATA</LG><Y>,</Y> <LG>SIZE</LG> <Y>)</Y><C>;</C>
<G>// Saves SIZE bytes from DATA to the file</G>

<G>// Used like:</G>
<Y>file</Y> f <Y>=</Y> <M>new_file</M><Y>(</Y> <C>"output.txt"</C> <Y>)</Y><C>;</C>
<Y>byte</Y> d<Y>[]</Y> <Y>=</Y> <C>"Hello H!"</C><C>;</C>
<M>file_save</M><Y>(</Y> <M>ref_of</M><Y>(</Y> f <Y>),</Y> d<Y>,</Y> <M>size_of</M><Y>(</Y> d <Y>) )</Y><C>;</C>
<G>// This saves "Hello H!" to output.txt</G>
</pre>

Mapping:
<pre>
<Y>file</Y> <M>map_file</M><Y>(</Y> <LG>PATH</LG><Y>,</Y> <LG>OPTIONAL_PATH_SIZE</LG> <Y>)</Y><C>;</C>
<G>// Memory-maps a file for fast access</G>
<G>// OPTIONAL_PATH_SIZE can be used if</G>
<G>//  the path is long and pre-measured</G>

<M>file_unmap</M><Y>(</Y> <LG>FILE_REF</LG> <Y>)</Y><C>;</C>
<G>// Unmaps the memory-mapped file</G>

<G>// Used like:</G>
<Y>file</Y> f <Y>=</Y> <M>map_file</M><Y>(</Y> <C>"large.dat"</C> <Y>)</Y><C>;</C>
<Y>byte ref</Y> data <Y>=</Y> f<C>.</C>mapped_bytes<C>;</C>
<G>// Now data points to the file contents</G>
<M>file_unmap</M><Y>(</Y> <M>ref_of</M><Y>(</Y> f <Y>) )</Y><C>;</C>
</pre>

Cleanup:
<pre>
<M>file_close</M><Y>(</Y> <LG>FILE_REF</LG> <Y>)</Y><C>;</C>
<G>// Closes an open (non-mapped) file</G>
<G>// Used like:</G>
<G>// file_close( ref_of( f ) );</G>

<M>delete_file</M><Y>(</Y> <LG>PATH</LG> <Y>)</Y><C>;</C>
<G>// Deletes a file from disk</G>
<G>// Can be used like:</G>
<G>// delete_file( "test.txt" );</G>
<G>// or</G>
<G>// delete_file( f.path );</G>
</pre>

### File/Folder Utilities
<pre>
<Y>flag</Y> <M>file_exists</M><Y>(</Y> <LG>PATH</LG> <Y>)</Y>
<G>// Check if file exists at PATH</G>

<Y>flag</Y> <M>folder_exists</M><Y>(</Y> <LG>PATH</LG> <Y>)</Y>
<G>// Check if folder exists at PATH</G>

<Y>n8</Y> <M>get_file_size</M><Y>(</Y> <LG>PATH</LG> <Y>)</Y>
<G>// Get size of file in bytes</G>

<M>create_folder</M><Y>(</Y> <LG>PATH</LG> <Y>)</Y>
<G>// Create a new folder at PATH</G>

<M>delete_folder</M><Y>(</Y> <LG>PATH</LG> <Y>)</Y>
<G>// Deletes an empty folder</G>
<G>// Won't do anything if not empty</G>
</pre>

### Directory Operations
<pre>
<Y>n2</Y> <M>get_entries</M><Y>(</Y> <LG>DIR</LG><Y>,</Y> <LG>ENTRIES</LG><Y>,</Y> <LG>MAX</LG><Y>,</Y> <LG>TYPE</LG> <Y>)</Y><C>;</C>
<G>// Gets directory entries, returns amount</G>
<G>// ENTRIES must be byte[][PATH_MAX_SIZE]</G>

<Y>n2</Y> <M>get_files</M><Y>(</Y> <LG>DIR</LG><Y>,</Y> <LG>ENTRIES</LG><Y>,</Y> <LG>MAX</LG> <Y>)</Y><C>;</C>
<G>// Gets only files from directory</G>

<Y>n2</Y> <M>get_folders</M><Y>(</Y> <LG>DIR</LG><Y>,</Y> <LG>ENTRIES</LG><Y>,</Y> <LG>MAX</LG> <Y>)</Y><C>;</C>
<G>// Gets only folders from directory</G>

<G>// Entry types:</G>
<C>entry_files</C>   <G>// Files only</G>
<C>entry_folders</C> <G>// Folders only</G>
<C>entry_any</C>		  <G>// Both files and folders</G>

<G>// Used like:</G>
<Y>byte</Y> entries<Y>[</Y> <C>100</C> <Y>][</Y> <C>PATH_MAX_SIZE</C> <Y>]</Y><C>;</C>
<Y>n2</Y> count <Y>=</Y> <M>get_files</M><Y>(</Y> <C>"."</C><Y>,</Y> entries<Y>,</Y> <C>100</C> <Y>)</Y><C>;</C>
<M>iter</M><Y>(</Y> i<Y>,</Y> count <Y>)</Y>
<C>{</C>
	<M>print</M><Y>(</Y> entries<Y>[</Y>i<Y>]</Y> <Y>)</Y><C>;</C>
	<M>print_newline</M><Y>()</Y><C>;</C>
<C>}</C>
</pre>

-------
### Basic Output
<pre>
<M>print</M><Y>(</Y> <C>"Hello, World!"</C> <Y>)</Y>
<G>// Buffer null-terminated bytes</G>
<G>// It will only display once it hits a '\n'</G>
<G>//  or when print_newline() is called:</G>

<M>print_newline</M><Y>()</Y>
<G>// Output a newline character</G>
<G>// Displays the buffered print</G>

<M>print_size</M><Y>(</Y> <LG>BUFFER</LG><Y>,</Y> <C>100</C> <Y>)</Y>
<G>// Output exactly 100 bytes</G>

### Formatted Output
Formatting in <H>H</H> is HTML-esque, where angle-brackets <G>"</G><<G>...</G>><G>"</G> are used.
<pre>
<M>format_print</M><Y>(</Y> <LG>FORMAT_BYTES</LG><Y>,</Y> <LG>ARGS...</LG> <Y>)</Y><C>;</C>
<G>// Prints formatted bytes to the terminal</G>

<G>// Example:</G>
<M>format_print</M><Y>(</Y> <C>"&lt;c:g&gt<>&lt;/c&gt;\n"</C><Y>,</Y> <C>"Value"</C> <Y>)</Y><C>;</C>
</pre>

#### Format Specifiers
| Tag | Description |
|-----|-------------|
| <code>&lt;&gt;</code> | Insert string argument |
| <code>&lt;/&gt;</code> | Insert path separator |

#### Color Tags
| Tag | Color | Tag | Color |
|-----|-------|-----|-------|
| <code>&lt;c:r&gt;</code> | <b>Red</b> | <code>&lt;c:dr&gt;</code> | <b>Dark Red</b> |
| <code>&lt;c:g&gt;</code> | <b>Green</b> | <code>&lt;c:dg&gt;</code> | <b>Dark Green</b> |
| <code>&lt;c:b&gt;</code> | <b>Blue</b> | <code>&lt;c:db&gt;</code> | <b>Dark Blue</b> |
| <code>&lt;c:y&gt;</code> | <b>Yellow</b> | <code>&lt;c:dy&gt;</code> | <b>Dark Yellow</b> |
| <code>&lt;c:m&gt;</code> | <b>Magenta</b> | <code>&lt;c:dm&gt;</code> | <b>Dark Magenta</b> |
| <code>&lt;c:c&gt;</code> | <b>Cyan</b> | <code>&lt;c:dc&gt;</code> | <b>Dark Cyan</b> |
| <code>&lt;c:w&gt;</code> | <b>White</b> | <code>&lt;/c&gt;</code> | <b>Default color</b> |

<b>Background colors:</b> Use <code>&lt;bg:_&gt;</code> instead of <code>&lt;c:_&gt;</code> for any color above<br>
<b>Reset background:</b> Use <code>&lt;/bg&gt;</code> to return to default

#### Style Tags
| Tag | Effect | Tag | Effect |
|-----|--------|-----|--------|
| <code>&lt;b&gt;</code> | <b>Bold start</b> | <code>&lt;/b&gt;</code> | <b>Bold end</b> |
| <code>&lt;u&gt;</code> | <b>Underline start</b> | <code>&lt;/u&gt;</code> | <b>Underline end</b> |
| <code>&lt;i&gt;</code> | <b>Italic start</b> | <code>&lt;/i&gt;</code> | <b>Italic end</b> |

### Format Bytes Function
<pre>
<Y>n4</Y> <M>format_bytes</M><Y>(</Y> <LG>OUT</LG><Y>,</Y> <LG>FORMAT</LG><Y>,</Y> <LG>ARGS...</LG> <Y>)</Y><C>;</C>
<G>// Formats text to OUT, outputs bytes written</G>
<G>// Used like:</G>
<Y>byte</Y> b<Y>[</Y> <C>256</C> <Y>]</Y><C>;</C>
<M>format_bytes</M><Y>(</Y> b<Y>,</Y> <C>"&lt;c:b&gt;<>&lt;/c&gt;"</C><Y>,</Y> <C>"H"</C> <Y>)</Y><C>;</C>
<G>// You don't have to use the size output</G>

<M>format_bytes_move</M><Y>(</Y> <LG>OUT</LG><Y>,</Y> <LG>FORMAT</LG><Y>,</Y> <LG>ARGS...</LG> <Y>)</Y><C>;</C>
<G>// Formats text and advances OUT ref</G>
</pre>

-------
## Terminal

### Terminal Colors
<pre>
<M>terminal_set_color</M><Y>(</Y> <LG>red</LG> <Y>)</Y>
<G>// Set text color</G>

<M>terminal_set_bg_color</M><Y>(</Y> <LG>blue</LG> <Y>)</Y>
<G>// Set background color</G>

<M>terminal_set_bold</M><Y>()</Y>
<M>terminal_set_italic</M><Y>()</Y>
<M>terminal_set_underline</M><Y>()</Y>
<G>// Apply text formatting</G>

<M>terminal_reset_format</M><Y>()</Y>
<G>// Reset all formatting</G>

<M>terminal_clear</M><Y>()</Y>
<G>// Clear the terminal screen</G>

<G>// Available colors:</G>
<LG>white</LG>, <LG>red</LG>, <LG>yellow</LG>, <LG>green</LG>,
<LG>cyan</LG>, <LG>blue</LG>, <LG>magenta</LG>, <LG>gray</LG>
<G>// Dark variants: dark_red, etc.</G>
</pre>

### Terminal Input
<pre>
<Y>byte ref</Y> <LG>input</LG> <Y>=</Y> <M>get_terminal_input</M><Y>()</Y>;
<G>// Get line of text from user</G>
<G>// Returns pointer to static buffer</G>
<G>// Buffer is reused on next call</G>
</pre>

-------
## Platform Abstraction

<H>H</H> automatically detects and adapts to the platform:
<pre>
<C>OS_LINUX</C> <G>// 1 if Linux, 0 if not</G>
<C>OS_WINDOWS</C> <G>// 1 if Windows, 0 if not</G>
<C>OS_NAME</C> <G>// Bytes: "Linux", "Windows"</G>

<C>SEPARATOR</C> <G>// "/" on Unix, "\\" on Windows</G>
</pre>

-------
## Utility Macros

### Default Arguments
<pre>
<M>DEFAULT</M><Y>(</Y> <LG>VAL</LG>, <LG>ARG</LG> <Y>)</Y>
<G>// Becomes ARG if provided, otherwise VAL</G>

<M>DEFAULTS</M><Y>( (</Y> <LG>TUPLE</LG> <Y>),</Y> <LG>ARGS...</LG> <Y>)</Y>
<G>// Used like:</G>
<C>#define</C> <M>foo</M><Y>(</Y> <LG>NAME</LG><Y>,</Y> <LG>ARGS...</LG> <Y>)</Y><G>\</G>
	<LG>NAME</LG><Y>(</Y> <M>DEFAULTS</M><Y>( (</Y> <C>1</C><Y>,</Y> <C>2</C><Y>,</Y> <C>3</C> <Y>),</Y> <LG>ARGS</LG> <Y>) )</Y>
<G>// foo( test ) -> test( 1, 2, 3 )</G>
<G>// foo( test, 7 ) -> test( 7, 2, 3 )</G>
<G>// foo( test, 7, 8 ) -> test( 7, 8, 3 )</G>
<G>// foo( test, 7, 8, 9 ) -> test( 7, 8, 9 )</G>
<G>// foo( test, 7,, 9 ) -> test( 7,, 9 )</G>
</pre>

### Compile-Time Helpers
<pre>
<M>COUNT_ARGS</M><Y>(</Y> <LG>A</LG><Y>,</Y> <LG>B</LG><Y>,</Y> <LG>C</LG> <Y>)</Y>
<G>// Returns 3</G>

<M>AS_BYTES</M><Y>(</Y> <LG>ANYTHING</LG> <Y>)</Y>
<G>// AS_BYTES( hello ) converts to "hello"</G>
<G>// AS_BYTES( ":)" ) converts to "\":)\""</G>

<M>CHAIN</M><Y>(</Y> <LG>L</LG><Y>,</Y> <LG>R</LG><Y>,</Y> <LG>MID</LG><Y>,</Y> <LG>ARGS...</LG> <Y>)</Y>
<G>// L arg1 R MID L arg2 R MID L arg3 R ...</G>
<G>// Can be used like:</G>
<G>// #define F( ARGS... ) CHAIN( ,, +, ARGS )
<G>// F( A, B, C ) converts to A + B + C

<M>COMMA_IF_ARGS</M><Y>(</Y> <LG>ARGS...</LG> <Y>)</Y>
<G>// Inserts comma only if ARGS exist</G>

<M>PASTE_IF_ARGS</M><Y>(</Y> <LG>CODE</LG><Y>,</Y> <LG>ARGS...</LG> <Y>)</Y>
<G>// Pastes CODE only if ARGS exist</G>

<M>REQUIRE_SEMICOLON</M>
<G>// Forces semicolon after macro use</G>
<G>// Prevents syntax errors</G>
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

### Byte Array Declaration
<pre>
<M>declare_bytes</M><Y>(</Y> <LG>buffer</LG><Y>,</Y> <M>KB</M><Y>(</Y> <C>1</C> <Y>) )</Y>;
<G>// Creates empty buffer[1000]</G>
<G>// And buffer_ref that's at [0]</G>

<M>declare_bytes</M><Y>(</Y> <LG>msg</LG><Y>,</Y> <C>32</C><Y>,</Y> <C>"Hello"</C> <Y>)</Y>;
<G>// Creates msg[32] = "Hello"</G>
<G>// And msg_ref that's after the 'o'</G>

<LG>msg</LG><Y>[</Y><C>0</C><Y>]</Y> = <C>'7';</C> <G>// msg is now "7ello"</G>
<LG>msg_ref</LG><Y>[</Y><C>0</C><Y>]</Y> = <C>'E';</C> <G>// msg is now "7elloE"</G>
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
<M>start</M>
<C>{</C>
	<M>print</M><Y>(</Y> <C>"Hello, World!\n"</C> <Y>)</Y><C>;</C>
	<M>out</M> <C>success;</C>
<C>}</C>
</pre>

### File Processing
<pre>
<M>start</M>
<C>{</C>
	<Y>file</Y> f <Y>=</Y> <M>open_file</M><Y>(</Y> input_bytes<M>[</M> <C>1</C> <M>]</M> <Y>)</Y><C>;</C>
	<M>if_nothing</M><Y>(</Y> f<C>.</C>handle <Y>)</Y>
	<C>{</C>
		<M>print</M><Y>(</Y> <C>"Failed to open file\n"</C> <Y>)</Y><C>;</C>
		<M>out</M> <C>failure;</C>
	<C>}</C>
	
	<Y>byte</Y> input<M>[ KB</M><Y>(</Y> <C>100</C> <Y>)</Y> <M>]</M><C>;</C>
	<Y>temp byte ref</Y> input_ref <Y>=</Y> input<C>;</C>
	<M>file_load</M><Y>(</Y> <M>ref_of</M><Y>(</Y> f <Y>),</Y> input <Y>)</Y><C>;</C>
	
	<Y>byte</Y> output<M>[ KB</M><Y>(</Y> <C>100</C> <Y>)</Y> <M>]</M><C>;</C>
	<Y>temp byte ref</Y> output_ref <Y>=</Y> output<C>;</C>
	
	<Y>check_input</Y><C>:</C>
	<C>{</C>
		<Y>temp byte</Y> val <Y>=</Y> <M>val_of</M><Y>(</Y> input_ref <Y>)</Y><C>;</C>
		<M>with</M><Y>(</Y> val <Y>)</Y>
		<C>{</C>
			<M>when</M><Y>(</Y> <C>'\0'</C> <Y>)</Y> <M>jump</M> <Y>input_eof</Y><C>;</C>

			<M>when</M><Y>(</Y> <C>' '</C><Y>,</Y> <C>'\t'</C><Y>,</Y> <C>'\n'</C><Y>,</Y> <C>'\r'</C> <Y>)</Y> <M>skip</M><C>;</C>

			<M>when</M><Y>(</Y> <C>'E'</C><Y>,</Y> <C>'e'</C> <Y>)</Y>
			<C>{</C>
				<M>val_of</M><Y>(</Y> output_ref <Y>) =</Y> <C>'3';</C>
				output_ref <Y>=</Y> output_ref <Y>+</Y> <C>1;</C>
				<M>skip</M><C>;</C>
			<C>}</C>

			<M>other</M>
			<C>{</C>
				<M>val_of</M><Y>(</Y> output_ref <Y>) =</Y> val<C>;</C>
				output_ref <Y>=</Y> output_ref <Y>+</Y> <C>1;</C>
				<M>skip</M><C>;</C>
			<C>}</C>
		<C>}</C>
		input_ref <Y>=</Y> input_ref <Y>+</Y> <C>1;</C>
		<M>jump</M> <Y>check_input</Y><C>;</C>
	<C>}</C>

	<Y>input_eof</Y><C>:</C>
	<M>file_close</M><Y>(</Y> <M>ref_of</M><Y>(</Y> f <Y>) )</Y><C>;</C>
	<M>print</M><Y>(</Y> output <Y>)</Y><C>;</C>
	<M>print_newline</M><Y>()</Y><C>;</C>
	<M>out</M> <C>success;</C>
<C>}</C>
</pre>

-------
## License

H is released under CC0 (Creative Commons Zero) - effectively public domain. FOSS forever.
 
-------
