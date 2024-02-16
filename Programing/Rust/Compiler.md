Rust is a compiled language, meaning that the source code is is read by a program called the compiler that builds the machine code(basically assembly in ones and zeros).
The compiler creates a file called the executable, which can be run even without having rust in the computer.
if i have a rust file  
```rust
fn main() {
	println!("Hello, world");
}
```
i can run
```shell
rustc build file
```
and ill have the executable of the function main added to the directory which i can run just by ./main
[[cargo]] allows easier work with directories and stuff, similar to [[poetry]] 