```note-blue
Here’s how it works: the program will generate a random integer between 1 and 100. It will then prompt the player to enter a guess. After a guess is entered, the program will indicate whether the guess is too low or too high. If the guess is correct, the game will print a congratulatory message and exit.
```

During the creation of this project we will learn about:

- let
- match
- methods
- associated functions
- using external crates


to obtain user input and the print the result as output we need to bring the io library into scope

- `use std::io;`

We create a variable to hold the user input 

- `let mut guess = String::new();`
	- `String::new` is a function that returns a new instance of a `String`, which is provided by the standard library and is a growable, UTF-8 encoded piece of text.
	- notice the "mut" keyword, variables in rust are default immutable so we must declare when we want the variable to be changeable 
	- the `::` syntax tells us that `new` is an ***associated function*** of the `String`type, this function is designated to the whole type not just an instance, some languages call this a *static method*


`//` starts comments btw line normal

We use the io type to read from the command line using
- `io::stdin().read_line(&mut guess)`
	- read_line takes in a line from the console and appends it to the string that was inputted
	- The string needs to be mutable
	- We use the & symbol to denote that we are passing the variable by reference, and, even though our variable is mutable, by default references are not, so we need to add the mut to the reference as well.
- the .expect() line takes the return of .read_line, which is of a "Result" type which contains an enum that describes whether the function ran ok or errored.

Print out the guess from the stored user input using:

`println!("You guessed {}", guess);`

This uses a placeholder to print out the variable within the string, {}

##### Loading in a dependency

We will load in the rand crate to get random number generator functionality

```
[dependencies]
rand = "0.8.3"
```

This tells cargo that we want to build with the rand crate as well as our main program

