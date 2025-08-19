# Functions, Control Flow & Decision Making

## Session Overview
**Welcome Back! Today we'll learn to make our code smart and reusable.**

Last session we learned to store data. Today we'll learn to:
- Write reusable functions
- Make decisions with if/else
- Handle different scenarios with match
- Repeat tasks with loops
- Handle errors safely

By the end, you'll build a salary calculator that handles different tax brackets!

---

## Part 1: Functions - Reusable Code Blocks

### What is a Function?
Think of a function like a recipe you can use over and over. Instead of writing the same steps repeatedly, you write them once and call the function whenever needed.

- Input: Ingredients (parameters)

- Process: Cooking steps (function body)

- Output: The finished dish (return value)

### Basic Function Syntax

```rust
fn function_name(parameter: type) -> return_type {
    // Function body
    return_value // or no semicolon for return
}
```

### Key Components
1. _fn_ **Keyword**: Declares a function.
2. **Function Name**: Uses snake_case (e.g., _function_name_).
3. **Parameters**: Defined in parentheses with name and type (e.g., _parameter: type_). Omit parentheses for no parameters.
4. **Return Type**: Specified after _->_. Use _-> ()_ or omit for no return (unit type _()_).
5. **Function Body**: Enclosed in curly braces _{}_. The last expression without a semicolon is implicitly _returned_, or use return explicitly.

### Examples

1. **Simple Function (No Parameters, No Return)**

```rust
fn greet() -> () {
    // Print greeting
    println!("Hello, world!") // No semicolon for return (implicit unit type)
}
```

2. **Function with Parameter and Return**

```rust
fn square(num: i32) -> i32 {
    // Calculate square
    num * num // No semicolon for return
}
```

3. **Function with Multiple Parameters**

```rust
fn add(a: i32, b: i32) -> i32 {
    // Add two numbers
    a + b // No semicolon for return
}
```

4. **Function with Explicit Return**

```rust
fn divide(x: f64) -> f64 {
    // Divide 100 by x
    return 100.0 / x // Explicit return with semicolon
}
```

5. **Calling a Function**

```rust
fn main() {
    greet() // Prints "Hello, world!"
    let squared = square(5) // Returns 25
    let sum = add(3, 4) // Returns 7
    let quotient = divide(2.0) // Returns 50.0
}
```
### Notes

- **Immutability**: Parameters are immutable by default. Use _mut_ (e.g., _mut parameter: type_) for mutability.
- **Ownership** and Borrowing: Use references (e.g., _&str, &i32_) to avoid taking ownership.
- **Implicit Return**: The last expression without a semicolon is returned. Use _return_ for early exits.
- **Visibility**: Functions are private by default. Add _pub_ (e.g., _pub fn_) for public access.



**Important:** No semicolon on the last line means "return this value"

### Quick Practice
```rust
fn calculate_tip(bill: f64, percentage: f64) -> f64 {
    bill * (percentage / 100.0)
}

fn main() {
    let bill = 50.0;
    let tip = calculate_tip(bill, 15.0);
    println!("Tip on ${}: ${:.2}", bill, tip);
}
```

---

## Part 2: Making Decisions with if/else

Decision making in Rust with if and else lets your program choose between different paths depending on whether a condition is true or false. You give Rust a test (like “is this number bigger than 10?”), and if the test is true, it runs one block of code; if not, it can run another block with else. This makes your program smarter because it can react differently based on the situation instead of always doing the same thing.

### Basic if Statement
```rust
fn main() {
    let balance = 100;
    
    if balance > 50 {
        println!("You have enough money!");
    } else {
        println!("Low balance warning!");
    }
}
```

### Multiple Conditions
```rust
fn check_account_status(balance: i32) {
    if balance > 1000 {
        println!("Premium account");
    } else if balance > 100 {
        println!("Standard account");
    } else if balance > 0 {
        println!("Basic account");
    } else {
        println!("Overdrawn account!");
    }
}

fn main() {
    check_account_status(1500);
    check_account_status(150);
    check_account_status(-10);
}
```

### if in Expressions
```rust
fn main() {
    let age = 17;
    let status = if age >= 18 { "adult" } else { "minor" };
    println!("Status: {}", status);
}
```

---

## Part 3: Pattern Matching with match

Pattern matching in Rust is a powerful way to check a value against different possible shapes or cases, and then run the code that matches. Instead of writing lots of _if/else_ statements, you can use _match_ to clearly list out every possibility. For example, a number can be checked to see if it’s 1, 2, or something else, and Rust will guide you to handle all cases so your program is safer and easier to read.

### Basic match Statement
```rust
fn describe_grade(grade: char) {
    match grade {
        'A' => println!("Excellent!"),
        'B' => println!("Good job!"),
        'C' => println!("Average"),
        'D' => println!("Needs improvement"),
        'F' => println!("Failed"),
        _ => println!("Invalid grade"),  // _ means "anything else"
    }
}

fn main() {
    describe_grade('A');
    describe_grade('F');
    describe_grade('X'); // Invalid grade
}
```

### match with Numbers
```rust
fn traffic_light(light: i32) -> &'static str {
    match light {
        1 => "Red - Stop!",
        2 => "Yellow - Caution!",
        3 => "Green - Go!",
        _ => "Broken light!"
    }
}

fn main() {
    println!("{}", traffic_light(1));
    println!("{}", traffic_light(3));
    println!("{}", traffic_light(5));
}
```

---

## Part 4: Loops - Repeating Tasks

Loops in Rust are a way to make your program repeat an action without writing the same code over and over. You can use a _loop_ to repeat forever until you tell it to stop, a _while_ loop to keep running while a condition is true, or a _for_ loop to go through a range of numbers or items in a collection. This makes your programs more powerful because they can handle tasks like counting, checking lists, or running until a certain condition is met—all automatically.

### while Loop
```rust
fn main() {
    let mut count = 1;
    
    while count <= 5 {
        println!("Count: {}", count);
        count += 1;  // Same as count = count + 1
    }
    println!("Done counting!");
}
```

### for Loop with Ranges
```rust
fn main() {
    // Print numbers 1 to 5
    for i in 1..=5 {
        println!("Number: {}", i);
    }
    
    // Print numbers 0 to 4
    for i in 0..5 {
        println!("Index: {}", i);
    }
}
```

### loop - Infinite Loop with break
```rust
fn main() {
    let mut counter = 0;
    
    loop {
        counter += 1;
        println!("Counter: {}", counter);
        
        if counter == 3 {
            break;  // Exit the loop
        }
    }
    println!("Loop ended!");
}
```

---

## Part 5: Basic Error Handling with Result

Basic error handling with Result in Rust means that instead of your program crashing when something goes wrong, a function can return a special value that says either _Ok_ (everything worked and here’s the result) or _Err_ (something failed and here’s the error). This way, you as the programmer can decide what to do next; like retry, show a message, or safely stop; making your program more reliable to work with.

### The Problem
```rust
fn divide(a: f64, b: f64) -> f64 {
    a / b  // What if b is 0? This gives infinity!
}
```

### Better Solution with Result
```rust
fn safe_divide(a: f64, b: f64) -> Result<f64, &'static str> {
    if b == 0.0 {
        Err("Cannot divide by zero!")
    } else {
        Ok(a / b)
    }
}

fn main() {
    match safe_divide(10.0, 2.0) {
        Ok(result) => println!("Result: {}", result),
        Err(error) => println!("Error: {}", error),
    }
    
    match safe_divide(10.0, 0.0) {
        Ok(result) => println!("Result: {}", result),
        Err(error) => println!("Error: {}", error),
    }
}
```

**Key Concept:** `Result<T, E>` represents either success (`Ok`) or failure (`Err`)

---

## Part 6: Hands-On Exercise - Salary Calculator

Let's build a Rust program that calculates the tax, net salary, and salary level of employees based on their gross salary

```rust
//Write a function calc_tax(salary: f64) -> f64 
// that returns 0.0 if salary ≤ 20,000.

//Extend calc_tax to return 1500 + 10% of amount above 
// 20,000 if salary ≤ 50,000.

//Extend it further: if salary ≤ 100,000, 
// return 3000 + 20% of amount above 50,000.

//Add a final case: if salary > 100,000, 
// return 13,000 + 30% of amount above 100,000.

// Function net_salary() to calculate net salary after tax deduction.
// We will return a Result type so we can handle invalid inputs 
// (like negative salaries).
// Ok(...) will return the net salary, Err(...) will return an error message.


//Write a function salary_level(salary: f64) -> &'static str that 
// returns "Entry", 
// "Mid", "Senior", or "Executive" depending on the salary range.




fn main() {




}

```

### Assigment!
Try modifying the calculator:
1. Change the tax brackets
2. Add a function to calculate monthly salary (divide by 12)
3. Add bonus calculation (+3000 for Entry, +5000 for mid, +8% for Senior, 10000 + 8% for Executive)
4. Add error messages for input of negative salaries

---

## Session Wrap-Up

### What We Accomplished Today
**Core Programming Skills:**
- Functions - Reusable code blocks with parameters and return values
- Decision Making - if/else for different scenarios
- Pattern Matching - match for handling multiple options cleanly
- Loops - Repeating tasks with while, for, and loop
- Error Handling - Using Result to handle failures safely

**Real-World Application:**
- Built a complete salary calculator
- Handled different tax brackets
- Used proper error handling
- Organized code into logical functions

### Why This Matters for Smart Contracts
- Functions organize contract logic into reusable pieces
- Conditional logic handles different transaction scenarios
- Pattern matching processes different asset types
- Loops handle batch operations
- Error handling prevents contract failures

### Preview of Next Session
**Collections & Data Organization**
- Storing multiple employees in lists (Vec)
- Fast lookups with HashMap
- Processing collections with iterators
- Reading/writing data to files
- Building a book inventory system

### Practice:

### Rust Practice Questions: Functions, Control Flow & Decision Making

**Part 1: Functions**

1. Write a function `divide(x: f64) -> f64` that returns `100.0 / x`.  
2. Write a function `double(num: i32) -> i32` that doubles the input.  
3. Write a function `is_even(num: i32) -> bool` that checks if a number is even.  
4. Write a function `welcome(name: &str)` that prints `"Welcome, <name>!"`.  



**Part 2: If/Else**

5. Write a function `check_number(num: i32)` that prints whether the number is positive, negative, or zero.  
6. Write a function `can_vote(age: i32)` that prints `"Eligible to vote"` if age ≥ 18, otherwise `"Not eligible"`.  
7. Write a function `check_even_odd(num: i32)` that prints `"Even"` if even, else `"Odd"`.  
8. Write a function `check_account(balance: i32)` that prints `"Premium"`, `"Standard"`, `"Basic"`, or `"Overdrawn"`.  



**Part 3: Match**

9. Write a function `print_number(num: i32)` that prints `"One"`, `"Two"`, or `"Other"` using `match`.  
10. Write a function `describe_grade(grade: char)` that prints messages for `'A'`, `'B'`, `'C'`, `'D'`, and `'F'`.  
11. Write a function `day_of_week(day: i32)` that returns `"Monday"` to `"Sunday"` or `"Invalid"`.  
12. Write a function `check_vowel(c: char)` that prints `"Vowel"` or `"Consonant"`.  



**Part 4: Loops**

13. Write a function `count_to_five()` that uses a `while` loop to print numbers 1–5.  
14. Write a function `print_evens()` that uses a `for` loop to print even numbers from 0–20.  
15. Write a function `print_chars(word: &str)` that uses a `for` loop to print each character in a string.  
16. Write a function `print_squares()` that uses a `for` loop with `0..=5` and prints the square of each number.  



**Part 5: Result & Error Handling**

17. Write a function `safe_divide(a: f64, b: f64) -> Result<f64, &'static str>` that returns an error if `b == 0`.  
18. Write a function `test_division()` that calls `safe_divide(10.0, 2.0)` and prints the result using `match`.  
19. Write a function `to_positive(num: i32) -> Result<i32, &'static str>` that returns an error if `num < 0`.  
20. Write a function `rounded_divide(a: f64, b: f64) -> Result<f64, &'static str>` that returns division rounded to 2 decimals.  
  







Remember: You're building the foundation for smart contract development. Every concept we learn applies directly to blockchain applications!
