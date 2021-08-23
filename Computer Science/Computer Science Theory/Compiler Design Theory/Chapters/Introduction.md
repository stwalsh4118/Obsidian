## Introduction

---
### 1.1 Language Processors
There is a gap in the ability for humans to communicate with machines in general. We try to remedy this by creating artificial languages that we humans can understand and write and that can be turned into a language that a machine can understand, that being a stream of bits that instructs the machine on what operations to do.

This task, the creation of this stream of bits from our human artificial language, is completed with the help of a <span class="purple">***language processor***</span>, meant to bridge the proverbial gap in language between man and machine.

There are two different types of said language processors, generally speaking; 

<span class="red">**Interpreters**</span> and <span class="blue">**Translators**</span>

<span class="red">Interpreters</span> accept an input program from a *source language* and subsequently perform the operations within the program directly.

<span class="blue">Translators</span>, of which there are two, compilers and assemblers, take the input program from the * source language* and transform them into the *object language*, generally the machine language for the given machine the translator is running on. Compilers translate high-level code and assemblers translate low-level code.

Common foundation of all compilers is based within Automata Theory.


---
### 1.2 A Naive Compiler Model

The process of transforming the written patterns of a programming language into the machine code representation is fairly complex to try and think of a "compiler" as a single boxed program, therefore its easier to think of it as split into multiple smaller parts that all work together.

A naive approach to this separation of the tasks of a compiler can lead us to the image of 3 boxes, the <span class="green">*lexical box*</span>, the <span class="orange">*syntax box*</span>, and the <span class="brown">*code generator*</span>, that work in a serial connection to process the source program into machine code. Each of these boxes also has access to a 4th box, containing tables of data relating to the process of compilation, for example there may be a *symbol table* which holds the information about each variable or identifier that is within your source program. 

```mermaid
flowchart LR
	id1(Source Program)
	id2(Lexical Box)
	id3(Syntax Box)
	id4(Code Generator)
	id5(Tables)
	id6(Machine Code)
	id1 --> id2
	id2 --> id3
	id3 --> id4
	id5 <--> id2 & id3 & id4
	id4 --> id6
	
```

<span class="green">**Lexical Box**</span>

The lexical box gets the bit patterns representing the strings that make up the source program.*** Its job is to separate those bits into the individual words that they represent.***

Here is an example of an input line that we could take into the Lexical Box:
$$\text{IFB1=13GOTO4}$$

The lexical box would take this input line and discern the different distinct strings within them that pertain to our source language, in this case it would tell you that the string contains the substrings, IF, B1, =, 13, GOTO, and 4. These strings are commonly called *tokens*

Each *token* contains two parts, a class part and a value part. For example the **B1** token may be of class "variable" and might contain the value which is the pointer to the symbol table entry for B1

We can equate the process that the Lexical Box does on our input as collecting the individual letters in the string into words and then finding the meaning of those words in the dictionary; the dictionary being the symbol table in our case.

<span class="orange">**Syntax Box**</span>

**The Syntax Box's job is to take the tokens and their order from the Lexical Box and transform them into an order that represents the order in which the programmer intends the operations to be carried out.**

Lets take the input:

$$\text{A + B * C}$$

The Syntax Box will take these 5 tokens and transform them into new entities, called *atoms*, which represent better the order in which we want the operations to happen, based on **syntactical rules**.

Output: 

$$\text{MULT(B, C, R1) ADD(A, R1, R2)}$$
 
 We see that we now have a different order of operations than the input, where the multiplication happens first and the addition happens afterwards.
 
 *Atoms*, like tokens, have a class and value part to them. Taking the MULT atom above, it might be of class "MULT" and have the value consisting of the three pointers to B, C, and R1. Within the compiler the atom would be represented by an integer representing the MULT class and the three pointers representing the value part.
 
 The syntactical rules alluded to above can be seen as analogous to the grammar associated with a natural language, like English, and the transformation from the Lexical Box output to the Syntax Box output can be seen as a translation between two natural languages, like English to German.
 
 
 <span class="brown">**Code Generator**</span>
 
 The code generators job is the take the atoms from the Syntax Box and expand them into machine operations that will carry out the same intent as the atoms. This expansion may change depending on certain conditions, for example, the current state of the run time, the table values being accessed in the atoms, the state of the registers, etc. 
 
 Take MULT(B, C, R1), if B is a floating point number then the code generator must expand the atom using floating point operations, or if B and C are different types, then it may expand the atom with type-conversion operations, etc.
 
 Sometimes producing efficient machine code may need an in-depth analysis of the current registers contents. This can be effective but good register management depends largely on the system you are compiling on.
 
 Compiler subactivites which concern themselves with the "meaning" of the symbols is called *semantic processing*, for example type definitions and array dimensions. Sometimes this semantic processing is carried out within a separate box between the Syntax Box and the Code Generator. 
 
 Some subactivites which are solely created to produce more efficient code, not necessarily needed to make functioning code, can be called *optimizations*.
 
 ---
 ### 1.3 Passes And Boxes
 
 The control flow between the boxes can be set up in multiple ways, four of those being: single-pass and three-pass, and two two-pass.
 
 - Single-pass:  Control is passed between the boxes when needed, one at a time symbols are recognized then generated into atoms then code.
 - Triple-pass: All symbols are generated from the source program then all atoms are generated then all code is generated
 - Double-pass: There could be a single-pass like interface between either the lexical box and syntax box or the syntax box and the code generator.

In general this pass concept is useful for a variety of reasons:


#### Logic of the language
Some part of the code may imply that it needs multiple passes within the compiler, for example if the code references an identifier before it is declared. This case would require at least 2 passes to generate the identifier declaration for it to be actually used. 


#### Code optimization

Code can be better optimized if it has the max amount of context about the program it is compiling, for example knowing where every declaration is and where values can be changed. This takes at least one pass for the optimization to be able to know this context. 

#### Memory considerations

Memory can be saved with multi-pass compilers since the memory from the previous pass can be used again in later passes.

---
### 1.4 The Run-Time Implementation

The compiler design relates itsself to the *output* that it wants to get from the *input*. To design the output we must plan the data structures and control mechanisms that exist at run-time to implement the features of the language. Like recursion, array memory handing, etc. 

---
### 1.5 Mathematical Translation Models
The compiler itsself is a translator but it itsself is not the thing doing the translation. The compiler is an interconnection of basic building block automata, that carry out the actual operations of the translation.

The design theory consists of two parts:

1. the mathematical study of these machines including
 
 
 
 
