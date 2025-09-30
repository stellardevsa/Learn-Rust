# Rust derive Traits in Soroban Smart Contracts

When building smart contracts in Soroban, you often need your custom structs (like Student, Teacher, etc.) to support extra functionality such as cloning, debugging, or comparing.

Rust provides the #[derive(...)] attribute to automatically implement traits for your structs, so you don’t have to write the boilerplate code yourself.

## Why Derive?

Instead of manually implementing traits, e.g.

```rust
impl Clone for Student {
    fn clone(&self) -> Self {
        Self {
            name: self.name.clone(),
            age: self.age,
            class: self.class,
            height: self.height,
        }
    }
}
```

### You can just write:

```rust
#[derive(Clone)]
pub struct Student {
    pub name: Symbol,
    pub age: u32,
    pub class: u32,
    pub height: u32,
}
```

### Common Derives in Soroban
#### 1. Clone

Allows you to create copies of a struct.

Needed when moving values in/out of Soroban storage.

### Example:

let s1 = student.clone();

#### 2. Debug

Lets you print structs using {:?}.

Very useful for testing and debugging.

Example:

println!("{:?}", student);

#### 3. PartialEq

Enables == and != comparisons.

Useful for checking values in tests.

Example:

assert_eq!(student1, student2);

#### 4. Eq

A stronger form of equality.

Used together with PartialEq.

Says that comparisons are total (no floating-point edge cases).

#### 5. Copy ️

Makes the type copyable without .clone().

Only works if all fields are also Copy (like u32, bool).

Not valid if your struct has String, Symbol, Vec, etc.

Rarely used in Soroban.

Standard Pattern for Soroban Structs

When working with Soroban contracts, the recommended derive set is:

```rust
#[contracttype]
#[derive(Clone, Debug, Eq, PartialEq)]
pub struct Student {
    pub name: Symbol,
    pub age: u32,
    pub class: u32,
    pub height: u32,
}
```

Why?

Clone -> copy values from storage

Debug -> print them during tests

Eq, PartialEq -> compare them directly in tests

Example Test with assert_eq!

```rust
#[test]
fn test_student_equality() {
    let s1 = Student {
        name: symbol_short!("Alice"),
        age: 18,
        class: 12,
        height: 160,
    };

    let s2 = s1.clone();

    assert_eq!(s1, s2); // works because of Eq + PartialEq
    println!("{:?}", s1); // works because of Debug
}
```

With these derives, your structs are easier to store, compare, debug, and test in Soroban smart contracts.
