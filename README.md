<!--

author:   André Dietrich
email:    andre.dietrich@ovgu.de
version:  1.0.0
language: de_DE
narrator: Deutsch Female

script:   https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js

@NASM:        15
@C_sharp:      1
@CPP:          7
@clang_CPP:   27
@visual_CPP:  28
@clang_C:     26
@visual_C:    29
@LISP:        18
@D:           30
@F_sharp:      3
@GO:          20
@Haskell:     11
@Java:         4
@JavaScript:  17
@LUA:         14
@NodeJS:      23
@Octave:      25
@ObjectiveC:  10
@Pascal:       9
@Perl:        13
@PHP:          8
@Prolog:      19
@Python:       5
@Python3:     24
@R:           31
@Ruby:        12
@Scala:       21
@Scheme:      22
@SQL_server:  16
@MySQL:       33
@Oracle:      35
@PostgreSQL:  34
@TCL:         32
@BASIC:        2
@ClientSide:  36
@Swift:       37
@BASH:        38
@ADA:         39
@Erlang:      40
@Elixir:      41
@OCaml:       42
@Kotlin:      43
@Brainfuck:   44
@Fortran:     45

@eval
<script>
var result = null;
var error  = false;

$.ajax ({
    url: "http://rextester.com/rundotnet/api",
    type: "POST",
    async: false,
    data: { "LanguageChoice": "@0",
            "Program": `{X}`,
            "Input": "",
            "CompilerArgs" : ""}
    }).done(function(data) {
        if (data.Errors == null) {
            result = data.Result;

            $('#Files').empty();

            if (data.Files != null) {
                for (var key in data.Files)
                {
                    var img_div = $(document.createElement('div'));
                    var img = $(document.createElement('img'));

                    img.attr('src', "data:image/png;base64," + data.Files[key]).height(600).width(700);

                    img.appendTo(img_div);
                    img_div.appendTo($('#Files@1'));
                }
            }
        } else {
            error  = true;
            result = data.Errors;
        }
    }).fail(function(data, err) {
        error = true;
        result = JSON.stringify(err);
    });

if (error)
    throw {message: result};
else
    result;
</script>

<div id="Files@1"> </div>
@end

-->

# rextester_template

Template for executing code snippets in 45 different programming languages.

If you are on github, then see the rendered version [here](https://liascript.github.io/course/?https://raw.githubusercontent.com/liaScript/rextester_template/master/README.md)

github-project:

                               --{{0}}--
Unfortunately, to be able to use rextester you will have to allow active mixed
mode content, but its worth it. To enable this setting in your browser, follow
the following steps.

**Firefox:**

1. enter about:config in your browser-url
2. search for "security.mixed_content.block_active_content"
3. double-click on it, to disable it (set to false)

**Chrome(ium)**

Click on the shield in upper right corner and load unsecure content, as it is
depicted below.

![enable mixed content](img/chrome.png)

## Ada

```ada
--GNAT 4.9.3

with Ada.Text_IO; use Ada.Text_IO;
procedure Hello is
begin
    Put_Line ("Hello, world!");
end Hello;
```
@eval(@ADA,0)

## Assembly

```assembly
;nasm 2.11.08

section .data
    hello:     db 'Hello world!',10    ; 'Hello world!' plus a linefeed character
    helloLen:  equ $-hello             ; Length of the 'Hello world!' string

section .text
	global _start

_start:
	mov eax,4            ; The system call for write (sys_write)
	mov ebx,1            ; File descriptor 1 - standard output
	mov ecx,hello        ; Put the offset of hello in ecx
	mov edx,helloLen     ; helloLen is a constant, so we don't need to say
	                     ;  mov edx,[helloLen] to get it's actual value
	int 80h              ; Call the kernel

	mov eax,1            ; The system call for exit (sys_exit)
	mov ebx,0            ; Exit with return code of 0 (no error)
	int 80h;
```
@eval(@NASM,0)

## Brainfuck

This program prints out the words Hello World!:

```brainfuck
1 +++++ +++               Set Cell #0 to 8
2 [
3     >++++               Add 4 to Cell #1; this will always set Cell #1 to 4
4     [                   as the cell will be cleared by the loop
5         >++             Add 4*2 to Cell #2
6         >+++            Add 4*3 to Cell #3
7         >+++            Add 4*3 to Cell #4
8         >+              Add 4 to Cell #5
9         <<<<-           Decrement the loop counter in Cell #1
10     ]                   Loop till Cell #1 is zero
11     >+                  Add 1 to Cell #2
12     >+                  Add 1 to Cell #3
13     >-                  Subtract 1 from Cell #4
14     >>+                 Add 1 to Cell #6
15     [<]                 Move back to the first zero cell you find; this will
16                         be Cell #1 which was cleared by the previous loop
17     <-                  Decrement the loop Counter in Cell #0
18 ]                       Loop till Cell #0 is zero
19
20 The result of this is:
21 Cell No :   0   1   2   3   4   5   6
22 Contents:   0   0  72 104  88  32   8
23 Pointer :   ^
24
25 >>.                     Cell #2 has value 72 which is 'H'
26 >---.                   Subtract 3 from Cell #3 to get 101 which is 'e'
27 +++++ ++..+++.          Likewise for 'llo' from Cell #3
28 >>.                     Cell #5 is 32 for the space
29 <-.                     Subtract 1 from Cell #4 for 87 to give a 'W'
30 <.                      Cell #3 was set to 'o' from the end of 'Hello'
31 +++.----- -.----- ---.  Cell #3 for 'rl' and 'd'
32 >>+.                    Add 1 to Cell #5 gives us an exclamation point
33 >++.                    And finally a newline from Cell #6
```
@eval(@Brainfuck,0)

The same program in minimised form:

```brainfuck
++++++++[>++++[>++>+++>+++>+<<<<-]>+>+>->>+[<]<-]>>.>---.+++++++..+++.>>.<-.<.+++.------.--------.>>+.>++.
```
@eval(@Brainfuck,1)

## C

```c
#include  <stdio.h>

int main(void)
{
    printf("Hello, world!\n");
    return 0;
}
```
@eval(@C,1)

## C++

```cpp
#include <iostream>

int main()
{
    std::cout << "Hello, world!\n";
}
```
@eval(@CPP,1)

## C-Sharp

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;

namespace Rextester
{
    public class Program
    {
        public static void Main(string[] args)
        {
            //Your code goes here
            Console.WriteLine("Hello, world!");
        }
    }
}
```
@eval(@C_sharp, 33)


## Elixir ![logo](https://avatars.githubusercontent.com/elixir-lang?s=128)<!-- height="22px" -->

```elixir
#Elixir 1.1.0

IO.puts "Hello, world!"
```
@eval(@Elixir, 33)

## LISP ![logo](https://upload.wikimedia.org/wikipedia/commons/6/64/Lisplogo_alien_256.png)<!-- height="21px" -->

```lisp
;gnu clisp 2.49

(print "Hello, world!")
```
@eval(@LISP, 33)


## Java


``` java
//'main' method must be in a class 'Rextester'.
//Compiler version 1.8.0_111

import java.util.*;
import java.lang.*;

class Rextester
{
    public static void main(String args[])
    {
        System.out.println("Hello, World!");
    }
}
```
@eval(@Java, 33)

## Python2

```python
for i in range(10):
    print i
```
@eval(@Python,0)

## Python3


## R

```R
require(stats) # for lowess, rpois, rnorm
plot(cars)
lines(lowess(cars))

plot(sin, -pi, 2*pi) # see ?plot.function

## Discrete Distribution Plot:
plot(table(rpois(100, 5)), type = "h", col = "red", lwd = 10, main = "rpois(100, lambda = 5)")

## Simple quantiles/ECDF, see ecdf() {library(stats)} for a better one:
plot(x <- sort(rnorm(47)), type = "s")
points(x, cex = .5, col = "dark red")
```
@eval(@R,1)

## SQL-Server

```sql
--Sql Server 2014 Express Edition
--Batches are separated by 'go'

select @@version as 'sql server version'
```
@eval(@SQL_server,1)


## Prolog

```prolog
exists(A, list(A, _, _, _, _)).
exists(A, list(_, A, _, _, _)).
exists(A, list(_, _, A, _, _)).
exists(A, list(_, _, _, A, _)).
exists(A, list(_, _, _, _, A)).

rightOf(R, L, list(L, R, _, _, _)).
rightOf(R, L, list(_, L, R, _, _)).
rightOf(R, L, list(_, _, L, R, _)).
rightOf(R, L, list(_, _, _, L, R)).

middle(A, list(_, _, A, _, _)).

first(A, list(A, _, _, _, _)).

nextTo(A, B, list(B, A, _, _, _)).
nextTo(A, B, list(_, B, A, _, _)).
nextTo(A, B, list(_, _, B, A, _)).
nextTo(A, B, list(_, _, _, B, A)).
nextTo(A, B, list(A, B, _, _, _)).
nextTo(A, B, list(_, A, B, _, _)).
nextTo(A, B, list(_, _, A, B, _)).
nextTo(A, B, list(_, _, _, A, B)).

puzzle(Houses) :-
    exists(house(red, english, _, _, _), Houses),
    exists(house(_, spaniard, _, _, dog), Houses),
    exists(house(green, _, coffee, _, _), Houses),
    exists(house(_, ukrainian, tea, _, _), Houses),
    rightOf(house(green, _, _, _, _), house(ivory, _, _, _, _), Houses),
    exists(house(_, _, _, oldgold, snails), Houses),
    exists(house(yellow, _, _, kools, _), Houses),
    middle(house(_, _, milk, _, _), Houses),
    first(house(_, norwegian, _, _, _), Houses),
    nextTo(house(_, _, _, chesterfield, _), house(_, _, _, _, fox), Houses),
    nextTo(house(_, _, _, kools, _),house(_, _, _, _, horse), Houses),
    exists(house(_, _, orangejuice, luckystike, _), Houses),
    exists(house(_, japanese, _, parliament, _), Houses),
    nextTo(house(_, norwegian, _, _, _), house(blue, _, _, _, _), Houses),
    exists(house(_, _, water, _, _), Houses),
    exists(house(_, _, _, _, zebra), Houses).

solution(WaterDrinker, ZebraOwner) :-
    puzzle(Houses),
    exists(house(_, WaterDrinker, water, _, _), Houses),
    exists(house(_, ZebraOwner, _, _, zebra), Houses).
```
@eval(@Prolog,1)
