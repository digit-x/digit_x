> Created on Sat Jan  1 16:52:19 2022 @author: Richie Bao - 西安建筑科技大学-规划/建筑/景观本科数字化系列课程

# Chapter 1-2: Running Hello, World! & Understanding program structure

## Chapter 1: Running Hello, World!

### 1.1 Technical requirements

<a href="https://sourceforge.net/projects/embarcadero-devcpp/"><img src="./imgs_C/014.png" height="auto" width="auto"  title="digit-x"></a> Dev-C++

<a href="https://www.codeblocks.org/"><img src="./imgs_C/015.png" height="auto" width=80 title="digit-x"></a> Code::Blocks

---

* Writing your first C program | Hello, world!

```c
#include <stdio.h>

int main()
{
  printf( "Hello, world!\n" );
  return 0;
}
```

### 1.2 Understanding the program development cycle | Creating, typing, and saving your first C program

* Interpreted VS Compiled

Interpreted(python)

<img src="./imgs_C/019.jpg" height="auto" width="auto"  title="digit-x">

    
Compiled(C,C++,C#, or Objective-C)

* Compile

<img src="./imgs_C/016.jpg" height="auto" width="auto"  title="digit-x">

<img src="./imgs_C/017.jpg" height="auto" width="auto"  title="digit-x">

<img src="./imgs_C/018.jpg" height="auto" width="auto"  title="digit-x">

<video height='auto' width=100% controls><source src="./video/what_is_compiler_short_and_simple_explanation_using_animation.mp4" height='auto' width='auto' title="digit-x" type='video/mp4'></video>

* compiler

Dev-C++

<img src="./imgs_C/020.jpg" height="auto" width="auto"  title="digit-x">

Code::Blocks

<img src="./imgs_C/021.jpg" height="auto" width="auto"  title="digit-x">

The GNU Compiler Collection (GCC) is an optimizing compiler produced by the GNU Project supporting various programming languages, hardware architectures and operating systems. The Free Software Foundation (FSF) distributes GCC as free software under the GNU General Public License (GNU GPL). GCC is a key component of the GNU toolchain and the standard compiler for most projects related to GNU and the Linux kernel. With roughly 15 million lines of code in 2019, GCC is one of the biggest free programs in existence.[4] It has played an important role in the growth of free software, as both a tool and an example.

When it was first released in 1987 by Richard Stallman, GCC 1.0 was named the GNU C Compiler since it only handled the C programming language.[1] It was extended to compile C++ in December of that year. Front ends were later developed for Objective-C, Objective-C++, Fortran, Ada, D and Go, among others.[6] The OpenMP and OpenACC specifications are also supported in the C and C++ compilers.[7][8]

GCC has been ported to more platforms and instruction set architectures than any other compiler, and is widely deployed as a tool in the development of both free and proprietary software. GCC is also available for many embedded systems, including ARM-based and Power ISA-based chips.

As well as being the official compiler of the GNU operating system, GCC has been adopted as the standard compiler by many other modern Unix-like computer operating systems, including most Linux distributions. Most BSD family operating systems also switched to GCC shortly after its release, although since then, FreeBSD, OpenBSD and Apple macOS have moved to the Clang compiler,[9] largely due to licensing reasons.[10][11][12] GCC can also compile code for Windows, Android, iOS, Solaris, HP-UX, AIX and DOS.[13] 

<a href="https://www.mingw-w64.org/downloads/"><img src="./imgs_C/022.jpg" height="auto" width="auto" title="digit-x"></a>

Mingw-w64 is an advancement of the original mingw.org project, created to support the GCC compiler on Windows systems. It has forked it in 2007 in order to provide support for 64 bits and new APIs. It has since then gained widespread use and distribution.

<img src="./imgs_C/025.jpg" height="auto" width="auto"  title="digit-x">

* Run and Verify

在Dev-C++下运行：

<img src="./imgs_C/023.jpg" height="auto" width="auto"  title="digit-x">

在Windows PowerShell下运行：

<img src="./imgs_C/024.jpg" height="auto" width="auto"  title="digit-x">


### 1.3 Writing comments to clarify the program later

```c
// comments.c
// Chapter 01
// Learn C Programming
//
// Compile with:
//
//    cc comments.c 
//

/* (1) A single-line C-style comment. */

/* (2) A multi-line
   C-style comment.   */

/*
 * (3) A very common way to 
 * format a multi-line
 * C-Style comment.
 */

/* (4) C-style comments can appear almost anywhere. */

/*5*/ printf( /* say hello */ "Hello, world!\n");

/*6*/ printf( "Hello, world!\n" ); /* yay! */

// (7) A C++ style comment (terminated by End-of-Line).

printf( "Hello, world!\n" ); // (8) Say hello; yay!

//
// (9) A more common way 
// of commenting with multi-line
// C++ style comments.
//

// (10) Anything can appear after //, even /* ... */ and
// more // after the first // but they will be 
// ignored because they are all in the comment.
```

* Adding comments to the Hello, world! program

```c
/*
 * hello2.c
 * My first C program with comments.
 * by <your name>
 * created <year>/<month>/<day> 
 *
 * compile with:
 *
 *   cc hello2.c 
 */

#include <stdio.h>

int main()
{
  printf( "Hello, world!\n" );
  return 0;
}

  /* eof */
  ```

## Chapter 2: Understanding Program Structure

### 2.1 Introducing statements and blocks

```c
#include <stdio.h>

int main()
{
  printf( "Hello, world!\n" );
  return 0;
}
```

* escape sequences

| Symbol  | Meaning  |
|---|---|
| \a  | Alert  |
| \b  | Backspace  |
| \f  | Form feed(or page advance)  |
| \n | New line  |
| \r  | Carriage return  |
| \t  | Horizontal tab  |
| \V  | Vertical tab  |
| \'  | Single quote  |
| \"  | Double quote  |
| \?  | Question mark  |
|  \\\ | Backslash itself  |

```c
// printingEscapeSequences.c
// Chapter 2
// Learn C Programming
//
// Demonstrate how escape sequences appear both
// in the source code and on the console.
//
// Compile with:
//
//    cc printingEscapeSequences.c -Wall -Werror -std=c11
//

#include <stdio.h>

int main( void )
{
  printf( "Hello, world without a new line" );
  printf( "Hello, world with a new line\n" );
  printf( "A string with \"quoted text\" inside of it\n\n" );

  printf( "Tabbed\tColumn\tHeadings\n" );
  printf( "The\tquick\tbrown\n" );
  printf( "fox\tjumps\tover\n" );
  printf( "the\tlazy\tdog.\n\n" );
  
  printf( "A line of text that\nspans three lines\nand completes the line\n\n" );
  
  return 0;
}

// <eof>
```

```
Hello, world without a new lineHello, world with a new line
A string with "quoted text" inside of it

Tabbed  Column  Headings
The     quick   brown
fox     jumps   over
the     lazy    dog.

A line of text that
spans three lines
and completes the line


--------------------------------
Process exited after 1.273 seconds with return value 0
Press any key to continue . . .
```

### 2.2 Understanding delimiters

1. Single delimiters:; and <space>
2. Paired, symmetric delimiters:<>,(),{},and ""
3. Asymmetric delimiters that begin with one character and end with another:# and <newline> and // and <newline>

* understanding whitespace

```c
#include<stdio.h>
int main(){printf("Hello, world!\n");}
```

### 2.3 Statements

1. Simple statements
2. Block statements
3. Complex statements
4. Compound statements

---

5. Preprocessor directive
6. Function statement
7. Functon call statement
8. Return statement
9. Block statement

---

10. Control statements
11. Looping statements
12. Expression statements

### 2.4 Functions

1. Function identifier
2. Function result type or return value type
3. Function block
4. Return statement
5. Function parameter list

* Exploring function identifiers

1. Camel-case
2. Snake-case(or underscore-separated)

* Exploring function return values

1. The return type of the function, given before the name of the function
2. The return value, which is of the same type as the return type

```c
#include <stdio.h>

void printComma()
{
  printf( ", " );
  return;
}

int main() 
{
  printf( "Hello" );
  printComma();
  printf( "world!\n" );
  return 0;
}
 ```
 ```
 Hello, world!
 ```

 ### 2.5 Passing in values with function parameters

 ```c
 #include <stdio.h>

void printGreeting( char* greeting , char* addressee )
{
  printf( "%s, %s!\n" , greeting , addressee );
}

int main() 
{
  printGreeting( "Hello" , "world" );
  printGreeting( "Good day" , "Your Royal Highness" );
  printGreeting( "Howdy" , "John Q. and Jane P. Doe" );
  printGreeting( "Hey" , "Moe, Larry, and Curly" );

  return 0; 
}
```
```
Hello, world!
Good day, Your Royal Highness!
Howdy, John Q. and Jane P. Doe!
Hey, Moe, Larry, and Curly!
```


### 2.6 Order of execution and function declaritions
 
 ```c
// hello7.c
// Chapter 2
// Learn C Programming
//
// Using function prototypes to call our functions in any
// order (before the compiler processes the function body).
// Sometimes this is called "top-down" function implementation.
//
// Compile with:
//
//    cc hello7.c -Wall -Werror -std=c11
//


#include <stdio.h>

// function prototypes

void printGreeting(    char* aGreeting , char* aName );
void printAGreeting(   char* greeting );
void printAnAddressee( char* aName );
void printAComma(   void );
void printANewLine( void );


int main() 
{
  printGreeting( "Hi" , "Bub" );

  return 0; 
}

void printGreeting( char* aGreeting , char* aName )
{
  printAGreeting( aGreeting );
  printAComma();
  printAnAddressee( aName );
  printANewLine();
}

void printAGreeting( char* greeting )
{
  printf( "%s" , greeting );
}

void printAnAddressee( char* aName )
{
  printf( "%s" , aName );
}

void printAComma( void )
{
  printf( ", " );
}

void printANewLine()
{
  printf( "\n" );
}

//  <eof>

```
```
Hi, Bub
```


