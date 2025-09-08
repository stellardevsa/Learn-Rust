
# Session 4: Structs, Methods & Code Organization

### View on Github
https://github.com/stellardevsa/Learn-Rust/edit/main/lesson4.md

## Session Overview

Welcome back!  

Last time, we worked with collections. Today we’ll go a step further and learn how to:

- Create our own custom data types with **structs**  
- Add actions/behaviours to them using **methods**  
- Keep our code clean and organised with **modules**  
- Write safer, more professional Rust code  

By the end, we’ll start building the foundation of an **Employee Management System**.

---
## Things to know

**Imagine a ``struct`` as a blueprint for an object**

```rust
struct Car {
    brand: String,
    speed: u32,
}
```

Now, if you want to give this car abilities (like "drive" or "stop"), you put them in an ``impl`` block.

**What ``self`` means**

Inside an ``impl`` block, ``self`` just means **"the thing we’re working on right now"**.

Think: "me, this particular car."

There are 3 common ways to use it:

**1. ``&self`` -> Borrow immutably**

This means: "I just want to look at the car, not change it."

```rust
impl Car {
    fn show_speed(&self) {
        println!("The {} is going {} km/h", self.brand, self.speed);
    }
}
```

- ``&self`` = "borrow the car to read it."
- Doesn’t take ownership, doesn’t change anything.

**2. ``&mut`` self -> Borrow mutably**

This means: "I want to change the car a bit, but I’ll give it back."

```rust
impl Car {
    fn accelerate(&mut self, amount: u32) {
        self.speed += amount;
    }
}
```

- ``&mut self`` = "borrow the car and modify it."

- Still doesn’t take ownership; just temporarily mutably borrows it.

**3. ``self`` -> Take ownership**

This means: "I’m taking the whole car, you won’t have it anymore after this."

```rust
impl Car {
    fn into_parts(self) -> (String, u32) {
        (self.brand, self.speed) // we break the car into pieces
    }
}
```

- ``self`` = "take the car completely."
- After calling this method, you can’t use that Car again (because ownership moved).

### Quick Analogy 

Think of ``self`` as a **car in a garage:**

- ``&self`` -> You look at the car.
- ``&mut self`` -> You tune up the car.
- ``self`` -> You drive the car away forever.

---

## Part 1: Structs – Grouping Related Data

### What is a Struct?

Think of a **struct** as a box that holds related information together.  

For example, instead of keeping separate variables for a person’s name, email, and salary, we can put them in **one box** called `Employee`.

### Defining and Using a Struct

```rust
struct Employee {
    id: u32,
    name: String,
    email: String,
    salary: f64,
    active: bool,
}

fn main() {
    let employee1 = Employee {
        id: 1,
        name: String::from("Alice"),
        email: String::from("alice@company.com"),
        salary: 75_000.0,
        active: true,
    };

    println!("Employee: {} earns ${}", employee1.name, employee1.salary);
}

```

**Key Idea:** A struct is your own custom type. Each instance is like a “record” or “profile” that groups info together.

## Updating Structs

```rust
fn main() {
    let mut employee = Employee {
        id: 2,
        name: String::from("Bob"),
        email: String::from("bob@company.com"),
        salary: 65_000.0,
        active: true,
    };

    // Change fields if struct is mutable
    employee.salary = 70_000.0;
    println!("Bob’s new salary: {}", employee.salary);
}
```
**Key Idea:** To change fields, the struct itself must be marked as ``mut`` .

## Part 2: Methods – Adding Behaviour

Structs hold **data**, but sometimes we want to attach **behaviour** (functions) to them. That’s where ``impl`` comes in.

**What does ``impl`` mean?**

``impl`` stands for **"implementation"**.
It’s like saying: “Here are the things that this struct can do.”

Imagine ``Employee`` is a game character:

The struct holds **stats** (name, salary, active).

The impl block adds **moves or skills** (display info, calculate bonus, give a raise).

### Example: Methods for Employee

```rust
struct Employee {
    id: u32,
    name: String,
    salary: f64,
    active: bool,
}

impl Employee {
    fn display(&self) {
        println!("{} earns ${}", self.name, self.salary);
    }

    fn annual_bonus(&self) -> f64 {
        if self.active {
            self.salary * 0.10
        } else {
            0.0
        }
    }

    fn give_raise(&mut self, percent: f64) {
        self.salary *= 1.0 + (percent / 100.0);
    }
}

fn main() {
    let mut employee = Employee {
        id: 1,
        name: String::from("Alice"),
        salary: 75_000.0,
        active: true,
    };

    employee.display();
    println!("Bonus: ${}", employee.annual_bonus());

    employee.give_raise(5.0);
    employee.display();
}
```

### Key Idea about ``self``:

- ``&self`` -> borrows the struct (just looks at data, no change)

- ``&mut`` self -> borrows mutably (changes the struct)

- ``self`` (no ``&``) -> takes ownership (moves the struct)

- ``impl`` is how we “teach” our struct new behaviours.

### Part 3: Organising Code with Modules

As projects grow, we don’t want one giant file. **Modules** help us group related code together.

```rust
mod employee_management {
    pub struct Employee {
        pub id: u32,
        pub name: String,
        pub salary: f64,
        pub active: bool,
    }

    impl Employee {
        pub fn new(id: u32, name: String, salary: f64) -> Employee {
            Employee { id, name, salary, active: true }
        }

        pub fn display(&self) {
            println!("{} earns ${}", self.name, self.salary);
        }
    }
}

fn main() {
    use employee_management::Employee;

    let emp = Employee::new(1, String::from("Alice"), 75_000.0);
    emp.display();
}

```

### Key Idea:

- ``mod`` = like a folder for your code
- ``pub`` = make something public so it can be used outside the module

## Part 4: Safer Struct Patterns

Rust encourages safe coding. For example, you can use ``Result`` to prevent creating invalid employees.

```rust
struct Employee {
    id: u32,
    name: String,
    salary: f64,
}

impl Employee {
    fn new(id: u32, name: String, salary: f64) -> Result<Employee, String> {
        if name.trim().is_empty() {
            Err("Name cannot be empty".to_string())
        } else if salary < 0.0 {
            Err("Salary cannot be negative".to_string())
        } else {
            Ok(Employee { id, name, salary })
        }
    }
}

fn main() {
    match Employee::new(1, "".to_string(), 50_000.0) {
        Ok(emp) => println!("Created employee: {}", emp.name),
        Err(err) => println!("Error: {}", err),
    }
}
```

### Key Idea:

- ``Ok(value)`` -> success

- ``Err(message)`` -> failure

This forces you to handle errors instead of ignoring them.


## Part 5: Mini Employee Manager

Now let’s build a simple employee manager.

```rust
use std::collections::HashMap;

#[derive(Debug)]
struct Employee {
    id: u32,
    name: String,
    salary: f64,
    active: bool,
}

struct EmployeeManager {
    employees: HashMap<u32, Employee>,
    next_id: u32,
}

impl EmployeeManager {
    fn new() -> EmployeeManager {
        EmployeeManager {
            employees: HashMap::new(),
            next_id: 1,
        }
    }

    fn add_employee(&mut self, name: String, salary: f64) {
        let employee = Employee {
            id: self.next_id,
            name,
            salary,
            active: true,
        };
        self.employees.insert(self.next_id, employee);
        self.next_id += 1;
    }

    fn list(&self) {
        for emp in self.employees.values() {
            println!("{:?}", emp);
        }
    }
}

fn main() {
    let mut manager = EmployeeManager::new();
    manager.add_employee("Alice".to_string(), 75_000.0);
    manager.add_employee("Bob".to_string(), 60_000.0);

    manager.list();
}
```

## Key Idea: 
A “manager” struct is just another struct that manages a collection of employees.

## Session Wrap-Up

### What We Learned

- Structs – make your own data types
- impl – give structs behaviours (methods)
- Modules – organise code neatly
- Error handling with Result – avoid bad data
- Mini Employee Manager – combining it all 

## Why It Matters

In real-world Rust (and smart contracts):

- Structs = accounts, tokens, contracts
- Methods = rules & logic
- Modules = clean structure
- Error handling = safety

Homework Challenge: Add a method to search employees by name!
