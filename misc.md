### Quick Definitions

- AJAX - Asynchronous JavaScript And XML
- callback - a single JS function which is executed in response to a triggering event or function
- MEAN stack: MongoDB, Express.js, Angular and Node.js
- Django Stack: Python / Django / Apache / MySQL
- Ruby Stack: Ruby / Ruby on Rails / RVM (Ruby Virtual Machine) /MySQL / Apache / PHP
- LAMP: Linux / Apache / MySQL / PHP

### The Issue with Floating Point Math
Q: Why don’t numbers like 0.1 + 0.2 add up to a nice round 0.3, and instead return a weird result like 0.30000000000000004?
A: Because internally, computers use a format (binary floating-point) that cannot accurately represent a number like 0.1, 0.2 or 0.3 at all.When the code is compiled or interpreted, your “0.1” is already rounded to the nearest number in that format, which results in a small rounding error even before the calculation happens.

### Signed and Unsigned Integers

- An unsigned integer is either zero or a positive number, signed can be negative
in the binary system, an unsigned integer containing n bits can have a value between 0 and 2n - 1 (which is 2n different values)
- Most modern computers store memory in units of 8 bits, called a "byte" (also called an "octet"). Arithmetic in such computers can be done in bytes, but is more often done in larger units called "(short) integers" (16 bits), "long integers" (32 bits) or "double integers" (64 bits). Short integers can be used to store numbers between 0 and 216 - 1, or 65,535. Long integers can be used to store numbers between 0 and 232 - 1, or 4,294,967,295. and double integers can be used to store numbers between 0 and 264 - 1, or 18,446,744,073,709,551,615.  
- When a computer performs an unsigned integer arithmetic operation, there are three possible problems which can occur:
  - if the result is too large to fit into the number of bits assigned to it, an "overflow" is said to have occurred. For example if the result of an operation using 16 bit integers is larger than 65,535, an overflow results.
  - in the division of two integers, if the result is not itself an integer, a "truncation" is said to have occurred: 10 divided by 3 is truncated to 3, and the extra 1/3 is lost. This is not a problem, of course, if the programmer's intention was to ignore the remainder
  - any division by zero is an error, since division by zero is not possible in the context of arithmetic.
- in signed magnitude, the digit furthest to the left (the sign bit) is used as a flag to identify positive or negative. this means you can only count half as far, but in both directions (pos and neg)

### Assembly Language

An assembly (or assembler) language,[1] often abbreviated asm, is a low-level programming language for a computer, or other programmable device, in which there is a very strong (but often not one-to-one) correspondence between the language and the architecture's machine code instructions. Each assembly language is specific to a particular computer architecture. In contrast, most high-level programming languages are generally portable across multiple architectures but require interpreting or compiling. Assembly language may also be called symbolic machine code.[2]
Assembly language is converted into executable machine code by a utility program referred to as an assembler. The conversion process is referred to as assembly, or assembling the source code. Assembly time is the computational step where an assembler is run.

Assembly language uses a mnemonic to represent each low-level machine instruction or opcode, typically also each architectural register, flag, etc. Many operations require one or more operands in order to form a complete instruction and most assemblers can take expressions of numbers and named constants as well as registers and labels as operands

### Traveling Salesman Problem

- an NP-complete problem, meaning the complexity class of decision problems for which answers can be checked for correctness, given a certificate, by an algorithm whose run time is polynomial in the size of the input (that is, it is NP) and no otherNP problem is more than a polynomial factor harder. An NP problem is one where a computer algorithm that verifies a solution can be created in polynomial time. an NP-Complete problem is NP, but also if you can solve it in polynomial time (called P) then all NP problems are P.
- in the problem, you’re given a list of cities and the distances between them and need to write an algorithm that returns the shortest possible route that visits every city exactly once and returns to the origin city

### Statically vs Dynamically Typed languages

Statically typed languages
- A language is statically typed if the type of a variable is known at compile time. For some languages this means that you as the programmer must specify what type each variable is (e.g.: Java, C, C++); other languages offer some form of type inference, the capability of the type system to deduce the type of a variable (e.g.: OCaml, Haskell, Scala, Kotlin)

- The main advantage here is that all kinds of checking can be done by the compiler, and therefore a lot of trivial bugs are caught at a very early stage.

Dynamically typed languages
- A language is dynamically typed if the type is associated with run-time values, and not named variables/fields/etc. This means that you as a programmer can write a little quicker because you do not have to specify types every time (unless using a statically-typed language with type inference). Example: Perl, Ruby, Python

- Most scripting languages have this feature as there is no compiler to do static type-checking anyway, but you may find yourself searching for a bug that is due to the interpreter misinterpreting the type of a variable. Luckily, scripts tend to be small so bugs have not so many places to hide.

- Most dynamically typed languages do allow you to provide type information, but do not require it. One language that is currently being developed, Rascal, takes a hybrid approach allowing dynamic typing within functions but enforcing static typing for the function signature.

### Pass by Reference vs Pass by Value

- It's a way how to pass arguments to functions. Passing by reference means the called functions' parameter will be the same as the callers' passed argument (not the value, but the identity - the variable itself). Pass by value means the called functions' parameter will be a copy of the callers' passed argument. The value will be the same, but the identity - the variable - is different. Thus changes to a parameter done by the called function in one case changes the argument passed and in the other case just changes the value of the parameter in the called function (which is only a copy). In a quick hurry:

- Java only supports pass by value. Always copies arguments, even though when copying a reference to an object, the parameter in the called function will point to the same object and changes to that object will be see in the caller. Since this can be confusing, here is what Jon Skeet has to say about this.

- C# supports pass by value and pass by reference (keyword ref used at caller and called function). Jon Skeet also has a nice explanation of this here.
C++ supports pass by value and pass by reference (reference parameter type used at called function). You will find an explanation of this below.

- When a parameter is passed by reference, the caller and the callee use the same variable for the parameter. If the callee modifies the parameter variable, the effect is visible to the caller's variable.

- When a parameter is passed by value, the caller and callee have two independent variables with the same value. If the callee modifies the parameter variable, the effect is not visible to the caller.

Example
- Say I want to share a web page with you. If I tell you the URL, I'm passing by reference. You can use that URL to see the same web page I can see. If that page is changed, we both see the changes. If you delete the URL, all you're doing is destroying your reference to that page - you're not deleting the actual page itself.

- If I print out the page and give you the printout, I'm passing by value. Your page is a disconnected copy of the original. You won't see any subsequent changes, and any changes that you make (e.g. scribbling on your printout) will not show up on the original page. If you destroy the printout, you have actually destroyed your copy of the object - but the original web page remains intact.

Sources:
https://en.wikipedia.org/wiki/Assembly_language,
http://kias.dyndns.org/comath/13.html,
https://stackoverflow.com/questions/588004/is-floating-point-math-broken,
https://stackoverflow.com/questions/1517582/what-is-the-difference-between-statically-typed-and-dynamically-typed-languages
