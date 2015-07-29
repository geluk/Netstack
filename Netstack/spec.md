﻿
Netstack Language Specification
=============

Netstack is a small, stack-based programming language.
It is based around the concept of functions taking arguments from
the stack and pushing their results onto it.

Syntax
=============

Functions
-------------
In Netstack, everything you write is a function. Literals are internally 
representedas functions that take no arguments and always return the same value.
A statement contains a sequence of functions that will get executed in a
left-to-right order, each function modifying the data currently on the stack.
When all functions have finished executing, the data left on the stack is the
result of the statement's execution. A REPL would usually print this result
to the console.

Aliases
-------------
An alias is a shorthand character sequence that can be used instead of a 
function call. The interpreter internally translates aliases to function
calls before executing the application. The Netstack language has four 
built-in aliases:

+ -> add
- -> subtract
* -> multiply
/ -> divide

Users can define any other alias that doesn't already exist

Special Aliases
-------------
A special alias is a special character sequence which
is used to denote data, such as a literal or a function binding.

The following literals are available:

Boolean:
	Any character sequence that is either "True" or "False".
Integer:
	Any character sequence consisting only of the digits 0-9.
String:,
	Any character sequence started and terminated by a quotation mark (`"`).
	Quotation marks may be escaped by prefixing them with a `\`.


Details
=============

Primitives
-------------
There are four primitive types in Netstack:
 - Integer
 - Boolean
 - Statement

Most of these should be fairly straightforward. Note that Integer specifies a
64-bit signed integer.
Statement, on the other hand, is more unique. It is a sequence of functions
which, when executed, will operate on the current stack. When encountered by the
parser, a statement will simply be pushed on the stack. Only when the end of the
application is reached is the current statement on the stack executed. 



The Stack
-------------
The stack is where Netstack stores all data relevant to the application.
It can only contain primitive types.

Examples
=============

The following statement:

    5 2 + exec

Will result in the following actions being taken:
 - Initialise an empty stack.
 - A new statement is encoutered
 - All aliases are converted into function calls
 - The end of the statement is encountered
 - The statement is pushed on the stack
 - The Exec function is called, which executes the statement currently on the stack
 - The statement currently on the stack is executed
 - The Add alias is called, which pops the first two primitives from the stack,
   adds them together, then pushes the results to the stack.
 - The statement ends
 - The end of the application is encountered
 - all data is taken from the stack, formatted, and printed


 Control statement:

("input is greater than three")
(
	read
	parseint 3 >
) if

Fibonacci sequence:

(
	# index 0
	(0) 
	(
		# index 1
		(1)
		(
			# any index above 1
			-- # subtract 1
			dup fib # acquire the value for this index
			swap # 
			--
			fib
			+
		)
		(dup 1 =) if
	)
	(dup 0 =) if
) .fib defn
8 fib


( ( 0 ) ( ( 1 ) ( -- dup fib swap -- fib + ) (dup 1 =) if ) (dup 0 =) if ) .fib defn
8 fib