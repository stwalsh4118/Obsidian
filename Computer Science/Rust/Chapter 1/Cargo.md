## Cargo

Create new projects with `Cargo new <file_name>`

Within the cargo.toml file that was created:

```toml
[package]
name = "hello_cargo"
version = "0.1.0"
authors = ["Your Name <you@example.com>"]
edition = "2018"

[dependencies]
```

Under the '[package]' tag we add metadata that configures our package/project

Under the '[dependencies]' tag we add dependencies that we want to use within our project

Build your project using `cargo build`, this creates a 'target' folder that houses your executable, along with other things, you can then run your executable using '.\target\debug\\<file_name>'

Or use `cargo run` to build and run your code

`cargo check` makes sure your code compiles but doesn't create an executable, this is much faster whenever you just want to make sure your code builds instead of actually wanting to run the executable

Running `cargo build --release` will build a release version of your program, with more optimizations so it will run faster. This build process will take longer but the code is more optimized for release, only do this when you are releasing the version.