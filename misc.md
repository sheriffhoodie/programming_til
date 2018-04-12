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
- in the problem, you’re given a list of cities and the distances between them and need to write an algorithm that returns the shortest possible route that visits every city exactly once and returns to the origin city\

Sources:
https://en.wikipedia.org/wiki/Assembly_language,
http://kias.dyndns.org/comath/13.html,
https://stackoverflow.com/questions/588004/is-floating-point-math-broken
