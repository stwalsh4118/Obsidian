Functions are declared with `fn`

Rust doesn't care where your function is defined within your program, as long as it *is* defined

Your functions can take in *parameters* as data to use within your function. Parameters must be [[Data Types|type]] annotated, and you can have as many parameters as you want.

```js
fn main() {
    another_function(5);
}

fn another_function(x: i32) {
    println!("The value of x is: {}", x);
}
```

Functions can contain both **statements** and **expressions**

Statements are lines of code that do not return any value, while expressions do return a value. ^807f16

`let y = 6` is a statement because in Rust, variable assignments do not return anything.

Use `{}` to create a new scope

```js
fn main() {
    let x = 5;

    let y = {
        let x = 3;
        x + 1
    };

    println!("The value of y is: {}", y);
}
```

Statements end in semi-colons, expressions do not. Adding an ending semi-colon to an expression will turn it into a statement, which doesn't return a value.

Functions that return values must annotate the type of the data that it should return

```js
fn five() -> i32 {
    5
}

fn main() {
    let x = five();

    println!("The value of x is: {}", x);
}
```

Functions also implicitly return the last expression within its block, they can also be returned earlier in the block using the return keyword.