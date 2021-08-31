A *scalar* type represents a single value

- [[#Integers|Integers]]
- [[#Floating-Point|Floating-point numbers]]
- [[#Boolean| Booleans]]
- [[#Characters|Characters]]

---


### Integers

An integer, or int, is a number without a fractional component

Integers can be either signed or unsigned, meaning they can have negative and positive integers, or only positive integers, respectively.

Integer types must have an explicit size

Integer types:

Length | Signed | Unsigned
-------|-------|---------
	8-bit|i8|u8
	16-bit|i16|u16
	32-bit|i32|u32
	64-bit|i64|u64
	128-bit|i128|u128
	arch. dependent|isize|usize
	
Integers can overflow in Rust, you must check for this error if you think that it could happen in parts of your program

---

### Floating-Point

There are two floating-point primitives in Rust, f32 and f64

f64 is the default in Rust

f32 is a single precision float
f64 is a double precision float

Basic numeric operations are available
- addition
- subtraction
- multiplication
- division
- modulo/remainder

---

### Boolean

True/False values

Holds 1 byte

Can be used in control flow

---
### Characters

The `char` type is denoted using single quotes, which differs from the `String` type which uses double quotes

Represents a Unicode Scalar Value, much more than normal ASCII