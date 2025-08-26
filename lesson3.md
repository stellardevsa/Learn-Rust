# Session 3: Collections & Data Organization

## Session Overview
Welcome back!  
So far, we’ve learned how to write **functions**, make **decisions** with `if`, and repeat actions with **loops**.  

But programs often need to handle **lots of data at once**—not just one number or one string. Today we’ll learn how to:  

- Store lists of items using **Vectors**  
- Store pairs of keys and values using **HashMaps**  
- Work with text using **Strings**  
- Save/load information in **files**  

👉 At the end, we’ll put everything together by building a **mini book inventory system**.  

---

## Part 1: Vectors – Lists That Can Grow  

A **Vector** is like a **list** or a **shopping cart**: you can put items in, take them out, and it can grow or shrink.  

```rust
fn main() {
    let mut numbers = Vec::new(); // empty list
    numbers.push(10);
    numbers.push(20);
    println!("{:?}", numbers); // [10, 20]

    let fruits = vec!["apple", "banana", "orange"];
    println!("{:?}", fruits);
}
```

Explanation

Vec::new() creates an empty list.

push() adds an item to the list.

println!("{:?}", ...) is used when printing lists or other complex data.

You can also create a list directly with vec![ ... ].

**Accessing items**

```rust
fn main() {
    let numbers = vec![1, 2, 3];
    println!("First: {}", numbers[0]); // direct access

    match numbers.get(5) { // safe access
        Some(num) => println!("Found: {}", num),
        None => println!("Nothing here!"),
    }
}
```

- ``numbers[0]`` gets the first element (but will **crash** if the index doesn’t exist).
- ``numbers.get(5)`` is safer—it returns ``None`` if the index is out of range

**Iterating (looping through a list)**

```rust
fn main() {
    let scores = vec![85, 92, 78];
    for score in &scores {
        println!("Score: {}", score);
    }
}
```
Here we say: “For every score inside the list scores, print it.”
The & means we’re **borrowing** the values instead of moving them out.


## Part 2: HashMap – Fast Lookups

A **HashMap** is like a **dictionary** or a phone book:

- You look up a key (e.g., a name)
- And get its value (e.g., a score).

```rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();
    scores.insert("Alice", 95);
    scores.insert("Bob", 87);

    match scores.get("Alice") {
        Some(score) => println!("Alice: {}", score),
        None => println!("Not found"),
    }
}
```
- ``HashMap::new()`` creates an empty map.
- ``.insert(key, value)`` stores the data.
- ``.get(key)`` looks it up and returns an Option

**Updating values (Word Counter Example)**

```rust
use std::collections::HashMap;

fn main() {
    let text = "hello world hello";
    let mut counts = HashMap::new();

    for word in text.split_whitespace() {
        let count = counts.entry(word).or_insert(0);
        *count += 1;
    }
    println!("{:?}", counts);
}
```
Here we:

1. Split the text into words.
2. For each word, check if it’s already in the HashMap.
3. If not, insert it with a starting value of 0.
4. Increase the count.

This is a classic way to count words in text.


## Part 3: Strings – Working with Text

- In Rust, there are two main types of strings:
- ``&str`` → fixed text (like "Hello"). It’s read-only.
- ``String`` → growable text. You can add to it.

```rust
fn main() {
    let mut message = String::from("Hello");
    message.push_str(", World!");
    println!("{}", message);

    let name = "Alice";
    let age = 25;
    println!("My name is {} and I am {}", name, age);
}
```
- ``String::from("Hello")`` creates a String.
- ``.push_str()`` adds more text.
- ``{}`` inside ``println!`` inserts values into the text.

**Useful String Methods**

```rust
fn main() {
    let text = "  Hello Rust  ";
    println!("Trimmed: '{}'", text.trim());
    println!("Upper: {}", text.to_uppercase());
    println!("Contains Rust? {}", text.contains("Rust"));
}
```
- ``.trim()`` removes spaces at the start and end.
- ``.to_uppercase()`` makes everything capital.
- ``.contains("Rust")`` checks if the text includes a word.


## Part 4: File I/O – Saving & Loading

Programs often need to **save data** so it’s not lost when they close.

**Writing to a file**

```rust
use std::fs;

fn main() {
    let scores = "85,92,78";
    fs::write("scores.txt", scores).expect("Unable to write");
}
```
- ``fs::write()`` saves text into a file.
- ``.expect()`` handles errors if something goes wrong.

**Reading from a file**

```rust
use std::fs;

fn main() {
    let data = fs::read_to_string("scores.txt").expect("Unable to read");
    println!("File content: {}", data);
}
```
- ``fs::read_to_string()`` loads text from the file.
- This is how programs **load data** back when you run them again.


## Part 5: Mini Project - Book Inventory

We’ll store books in a **HashMap**, where the **key** is the **ISBN** and the value is a Book struct with details.

```rust

use std::collections::HashMap;

#[derive(Debug)]

fn main() {

    // Add books
    // Look up a book
    // List all books


```

**Your challenge**

- Write a function to sell a book (reduce quantity).
- Write a function to check which books are low stock.

**Wrap-Up**

Today we learned:
- Vectors → lists of items
- HashMaps → key-value data
- Strings → working with text
- File I/O → saving and loading
- Built a book inventory system

Next time: **Structs, Methods & Organizing Code**






