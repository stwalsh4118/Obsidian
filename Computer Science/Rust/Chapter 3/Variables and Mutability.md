By default variables are immutable

```
fn main() {
    let x = 5;
    println!("The value of x is: {}", x);
    x = 6;
    println!("The value of x is: {}", x);
}

```

This code above fails to compile since you are trying to change the value of an immutable variable

If we change the above to be `let mut x = 5` then the variable x will be mutable and we can change its value later

We can declare constant variables using the `const` keyword.

`const MAX_POINTS: u32 = 100_000;`

A constant is always immutable and can be declared in any scope, including global. Every constant must have its [[Data Types|value type]] annotated.

---
#### Shadowing

We can create a *new* variable using the same name by shadowing

```
let x = 5;
let x = x + 1;
let x = 2 * x;
```

We can change the value of the x variable by using the `let` keyword again but keeping the same variable name.

Using this shadowing construct we can even change the types of the variable, which differs from mutability

```
let spaces = "   ";
let spaces = spaces.len();
```

This lets us change the string `"   "` into a number that represents the length of the string that was in the variable "spaces" before.

We can't do this using `mut` because mut only allows us to change the value of the variable, not the [[Data Types|type]].