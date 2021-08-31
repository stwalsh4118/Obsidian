Compound types can group multiple values into one type

Two primitive compound types

- [[#Tuples]]
- [[#Arrays]]

### Tuples

Tuples bind multiple different values into one variable

Tuple values do not have to be the same type

`let tup = (500, 6.4, 1)`

We can destructure tuples into individual variables like so

`let (x, y, z) = tup;`

The above takes the tuple values from "tup" and binds them to the variables x, y, and z in order of their index in the tuple "tup"

We can also use the . operator to get values from the tuple based on the index of the value we want to get, index is zero based 

```
let five_hundred = tup.0;
let six_point_four = tup.1;
let one = tup.2;
```

### Arrays

Unlike tuples every element in an array must have the same type

Arrays have fixed length

`let a = [1, 2, 3, 4, 5]`

Arrays allocate data on the stack instead of on the heap

Annotate the arrays type like so:

`let a: [i32; 5] = [1, 2, 3, 4, 5]`

You can initialize an array with the same value in every slot like so:

`let a = [3; 5]`

This will generate an array that looks like this `[3, 3, 3, 3, 3]`

Access arrays using normal array index syntax:

`let first_element = a[0]`

If you try to access an index that is past the length of the array, Rust will throw a runtime error and exit the program