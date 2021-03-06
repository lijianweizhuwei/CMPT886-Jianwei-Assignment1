## SWAR

@(CS)

#### Direction

Transform one instruction on irregular size of vectors to a bunch of instructions on regular size of vectors which are supported by CPUs.

With notion of SWAR.

If for Parabix, we can focus on several specific type of size
* 128-bit SIMD <32 x i4>, <64 x i2>, <128 x i1>
* 256-bit SIMD <64 x i4>, <128 x i2>, <256 x i1>

##### 1. Concept of SWAR
The basic concept of SWAR, SIMD Within A Register, is that operations on word-length registers can be used to speed-up computations by performing SIMD parallel operations on n k/n-bit field values.

##### 2. [Types of Operations](http://www.phys.aoyama.ac.jp/~w3-furu/aoyama+/Tech_notes/adaptor_doc/Users_Guide.pdf)
* Polymorphic Operations (data type independent) e.g.: AND, OR, ...
* Partitioned Operations (not independent) e.g.: + - x /
* Communication & Type Conversion Operations
* Recurrence Operations (Reductions, Scans, etc.)
* Masking Operations

##### 3. [Ways of Tranformation](https://www.tldp.org/HOWTO/Parallel-Processing-HOWTO-4.html)
1. Partitioned Instructions (hardware support)
2. ***Unpartitioned Operations With Correction Code***
3. ***Controlling Field Values***

#### Details
##### 1. Unpartitioned Operations With Correction Code
###### 1.1 Add
* Challenge: Overflow
* Solution:
	* Mask out high bit of each area
	* Perform addition as **result1**
	* XOR (Add high bits)
	* Mask high bits **result2**
	* Add two results with XOR
* [Example](https://coursys.sfu.ca/2016sp-cmpt-886-g2/pages/SWAR-Example1/view)

##### 2. Controlling Field Values
###### 2.1 Add
* Challenge: Overflow
* Solution: Add one bit in front of each vector for the result

e.g.: <4 x i7> x + <4 x i7> y
((x + y) & 0x7f7f7f7f)
![Alt text](../image/project_swar_1.jpeg)
