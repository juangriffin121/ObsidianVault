```rust
use std::io;
use std::fs;
use rand::seq::SliceRandom;

fn main() {
    let contents = fs::read_to_string("data/palabras_sin_acento.txt")
        .expect("Should have been able to read the file");
    let words: Vec<&str> = contents.split('\n').collect();
    let max_tries: i32 = 7;
    let mut want_to_play: bool = true;
    while want_to_play {
        play(&words, &max_tries);
        want_to_play = false;
    };
}

fn check_guess(guess: char, learned_word: String, word: &str) -> (String, bool) { 
    let mut changed_word: String = ""
        .to_string();
    for (index, c) in word.chars().enumerate() {
        if guess == c {
            changed_word.push(c);
        } else {
            let known_char: char = learned_word.chars()
                .nth(index)
                .expect("should have found char at that index");
            changed_word.push(known_char);
        };
    };
    println!("{changed_word:?}");
    let failed: bool = changed_word == learned_word;
    (changed_word, failed) 
}

fn play(words: &Vec<&str>, max_tries: &i32) {
    let mut tries: i32 = 0;
    let mut won: bool = false;
    let mut lost: bool = false;
    let mut failed: bool;
    let mut rng = rand::thread_rng();
    let word: &str = words.choose(&mut rng)
       .expect("Should have gotten a word");

    let length = word.chars().count(); 
    let mut learned_word: String = String::from_utf8(vec![b'_'; length]).unwrap();
    println!("{learned_word:?}");
    
    while !won && !lost {
        (learned_word, failed) = make_guess(word, learned_word);
        if failed {
            tries += 1;
        };
        println!("tries = {tries:?}");
        won = learned_word == word;
        lost = tries > *max_tries - 1;
    }
    if won {
        println!("you won");
    } else {
        println!("you lost, your word was: {word:?}");
    }   
}

fn make_guess(word: &str, learned_word: String) -> (String, bool){
    println!("make a guess");
    let mut input = String::new();
    io::stdin().read_line(&mut input)
        .expect("Failed to read line");
    let guess: char = input.chars().next()
        .expect("should have gotten char"); 
    check_guess(guess, learned_word, word)
}
```

```rust
let lines: Vec<&str> = multiline_string.split('\n').collect();
```

We are declaring a variable "lines", of type Vec<&str>:
(a Vector (
	array (
		all elements the same
	) allowed to grow or shrink in size
) of String slices, references to strings, not the string itself)
split ~python
collect is a method of iterators and returns the Vec

```rust
let contents = fs::read_to_string("data/palabras_sin_acento.txt")
        .expect("Should have been able to read the file");
```
fs imported as std::fs for "file system" has a function read_to_string, which takes in a string with a file path and returns a file as a Result<some_file_type, Err>, the expect method is used to take the Ok type of the struct Result and if the value gets anything in runtime that isn't that type it will panic.
Example
```rust
let r: Result <String, E> = try_to_get_string()
let value = r.expect("Failed to get value from result");
```
value will be of type String

when feeding references to a function you do this
```rust
play(&words, &max_tries);

fn play(words: &Vec<&str>, max_tries: &i32) {
	code
}
```
the function should expect references to the type of objects you feed them

Loops, if statements and stuff like that are done with curly braces
```rust
if condition {
	code_if_true
} else if second_condition {
	code_if_second_condition
} else {
	code_if_both_false
}

for item in iterator {
	code
}

while condition {
	code_that_changes_condition_at_some_point
}
```

word is a reference to str, the chars() method returns an iterator of its chars, and count counts the items in the iterator.
```rust
let length = word.chars().count();
```

Create a string of underscores of the same length as the word, the from_utf8 classmethod of String takes in a Vec of byte chars and returns a String, and the vec! macro creates a vector, in this case it made one with the information of the repeated value and the number of repeats, but it can also be done listing all the values.
```rust
let mut learned_word: String = String::from_utf8(vec![b'_'; length]).unwrap();
```

rusts boolean algebra symbols
```rust
while !won && !lost
```

how to receive tuples
```rust
(learned_word, failed) = make_guess(word, learned_word);
```


max_tries is a reference to a value in that function (max_tries: &i32 and its fed to it like &max_tries from the function that fed it that had the actual value stored in max_tries)
the * operator takes in a reference to a value and returns the value itself, the type of \*max_tries is i32.
```rust
lost = tries > *max_tries - 1;
```

initialize an empty String, then i pass its mutable reference to the io::stdin().read_line() to turn it into what the user inputed 
```rust
let mut input = String::new();
io::stdin().read_line(&mut input)
    .expect("Failed to read line");
```

since the string should actually be a single char, i pick the first one by creating the chars iterator from it and calling its next() method.
```rust
let guess: char = input.chars().next()
    .expect("should have gotten char");
```

```rust
io::stdin().read_line(&mut input) 
	.expect("Failed to read line");
```
