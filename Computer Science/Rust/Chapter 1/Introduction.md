## The Anatomy of a Rust Program
```js
fn main() {
	println!("Hello World");
}
```

Designate [[Functions|functions]] with fn

The main function is the first code that runs in every Rust program

`println!("Hello World")`

This line prints "Hello World" to the terminal, note the '!' this means its running a Rust macro not a function.

End [[Functions#^807f16|statements]] with semi-colons

Compile your simple program using rustc `<file-name>`

For more complicated programs, with dependencies and what not, you will need to use [[Cargo]] to help you with the complicated build processes.