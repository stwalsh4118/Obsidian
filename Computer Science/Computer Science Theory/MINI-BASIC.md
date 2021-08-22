# MINI-BASIC Rundown

#### General Form of a MINI-BASIC program

A program contains a sequence of lines with line indicators at the front of the line

Statements are executed in order by their line number unless control-flow is taken over by another statement


```BASIC
10 LET X = 12
20 IF X > 10 GOTO 30
30 END
```



### NUMBERS

A number can either be written as a sequence of decimal digits with or without the decimal point or with or without a sign, e.g.:

```BASIC
3.4712
-1234
2.
```

Or can be written in exponential notation like so

```BASIC
1234E-11
12.34E-9
.0000000001234E0
```

### VARIABLES

Denoted by a single letter or a single letter followed by a single digit

```BASIC
X
Y1
E3
Q0
```


### ARITHMETIC EXPRESSIONS

Supports these operators

```BASIC
+ addition
- subtraction
* multiplication
/ division
^ exponentiation
```

###### Order of operations

1. Operations within parentheses are carried out before the parenthesized quantity is used in further computations
2. Exponentiations are done before multiplications and divisions which in turn are done before additions or subtractions.

E.X.
```basic
A + (B/(C+D)) * F ^ G ^ (H + B) + C
  7   2   1    6   3  5     4     8
  
  The operations above are done in the specified order
```


### STATEMENTS	

##### Assignment statement

```basic
LET <variable>  = <arithemetic expression>

e.x.

LET X1 = Y1 + 12.6 * Z + X1
```

##### GOTO 	Statement

Transfers control-flow and goes to the specified line

```basic
GOTO <line number>

e.x.
GOTO 75
```


##### Conditional Statement

```basic
IF <expression> <relational operator> <expression> GOTO <line number>

e.x.

IF X > Y GOTO 12
IF Z + (X * Y) = X1 + 12 GOTO 75
```

###### Relational Operators

```basic
=  equal
<> not equal
<  less than
<= less than or equal
>  greater than
>= greter than or equal
```

##### FOR Statement and NEXT Statement

Used to set up program loops

Two general forms

```basic
FOR <variable> = <expression 1> TO <expression 2> STEP <expression 3>
FOR <variable> = <expression 1> TO <expression 2>

Second example is same as the one above except the "STEP" is assigned to 1
```

For loop is enclosed by a NEXT Statement
```note-gray
The variable for the NEXT statement must be the same as the variable for the FOR statment
```

```basic
FOR X = 1 TO 100 STEP 5
	LET W = X + Y + Z
	LET Z = X * Y
NEXT X
```

Loops can be nested

##### GOSUB and RETURN Statements

Subroutines are defined and called using the GOSUB and RETURN statements

```basic
GOSUB <line number>
```

Control is transferred to the specified line number and the subsequent statements are executed until a RETURN statement is reached

After subroutine completion control is transferred back to the line directly after where the GOSUB was called.

Subroutines can ***ONLY*** be exited by the RETURN statement

A GOSUB can call another GOSUB within itsself but can not call the GOSUB it is currently inside meaning ***NO RECURSION***

##### END Statement

A program must contain a single END statement which must be the highest numbered statement, this is where the program exits.


##### REM Statement

Used for comments, all content of the line is ignored by the compiler but the line number can be used for GOTO, IF, and GOSUB statements

