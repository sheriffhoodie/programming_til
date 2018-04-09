## Bit Manipulation

Bitwise operations are operations that directly manipulate bits (binary representations of all data in a computer).

- to quickly read binary, multiply each binary digit by two to the power of its place number, always starting with 2^0. Then add all the results together
- in python, can represent binary numbers by adding “0b” in front of any binary number, than can operate on it, ex. 0b1, 0b10, 0b11, 0b100, 0b101, 0b110, 0b111, 0b1000
- Shift Operators - shift the bits of a number over by a designated number of slots
can also use these as a shorthand expression to slide masks into place at a particular bit.
  - ex. mask = (0b1 << 9) to create a tenth bit mask for a number
  - left bit shift (<<) ex. 0b00001 << 2 == 0b00100
  - right bit shift (>>) ex. 0b1100 >> 2 == 0b11

- AND (&) operator - compares two numbers on a bit level and returns a number where the bits of that number are turned on (meaning are 1’s) if the corresponding bus of both numbers are 1
  - ex. a = 00101010 (42)
  - ex. b = 00001111 (15)
  - a & b = 00001010 (10)
- in this example, the 2’s bit and the 8’s bit are the only bits that are on in both numbers so the intersection only contains those
- using the & operator can only result in a number that is less than or equal to the smaller of the two values
- Examples: 0 & 0 = 0, 0 & 1 = 0, 1 & 0 = 0, 1 & 1 = 1

- OR (|) operator - compares two numbers on a bit level and returns a number where the bits of that number are turned on if either of the corresponding bus of either number are 1
  - ex. a = 00101010 (42)
  - ex. b = 00001111 (15)
  - a | b = 00101111 (1 + 2 + 4 + 8 + 32 = 47)
- using the | operator can only create results that are greater than or equal to the larger of the two integer inputs
- Examples: 0 | 0 = 0, 0 | 1 = 1, 1 | 0 = 1, 1 | 1 = 1

- XOR (^) “exclusive or” operator - compares two numbers on a bit level and returns a number where the bits of that number are turned on if either of the corresponding bits of the two numbers are 1, but not both
  - ex. a = 00101010 (42)
  - ex. b = 00001111 (15)
  - a ^ b = 00100101 (1 + 0 + 4 + 0 + 0 + 32 = 37)
- if a bit is off in both numbers, it stays off. XOR-ing a number with itself will always result in 0
- Examples: 0 ^ 0 = 0, 0 ^ 1 = 1, 1 ^ 0 = 1, 1 ^ 1 = 0

- NOT (~) operator - flips all the bits in a single number, which is equivalent to adding one to a number and then making it negative
- bit mask - a variable that helps with bitwise operations by turning specific bits on or off or just collecting data from an integer about which bits are on or off
```python
- num  = 0b1100
	mask = 0b0100
	desired = num & mask
	if desired > 0:
 	 print "Bit was on”
```
- can also use masks to turn a bit on in a number using the | operator; this will turn a corresponding bit on if it is off and leave it on if it is already on
  - a = 0b10111011
  - mask = 0b100
  - desired = a | mask (result = 0b10111111)
- can use XOR (^) operator to flip bits in a given binary number
  - ex. a = 0b110
  - mask = 0b111
  - desired = a ^ mask = 0b001
