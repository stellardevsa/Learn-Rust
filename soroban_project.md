## Soroban Project Assignment: Library Management Smart Contract (Deadline 15 October 11:59PM CAT)

**Objective:**
Create a **Library smart contract** using Soroban where users can store and manage a collection of books.

---

### What Your Contract Should Do

1. Define a `Book` struct with:

   * `title` (Symbol)
   * `author` (Symbol)
   * `year` (u32)

2. Define a `Library` struct containing a `Vec<Book>`.

3. Implement the following contract functions:

   * `initialize(env: Env)` → Creates an empty library and stores it in contract storage.
   * `add_book(env: Env, title: Symbol, author: Symbol, year: u32)` → Adds a new book.
   * `remove_book(env: Env, title: Symbol)` → Removes a book by title.

     * **Hint:** To remove, you’ll first need the **index** of the book. Use

       ```rust
       books.iter().position(|book| book.title == title)
       ```

       Here `|book| book.title == title` is a **closure** (like an inline function).
   * `find_book(env: Env, title: Symbol) -> Option<Book>` → Finds and returns a book if it exists.

     * **Hint:** To search, you can use:

       ```rust
       books.into_iter().find(|book| book.title == title)
       ```

       Again, the `|book| ...` part is a closure telling Rust *how* to check each element.
   * `list_books(env: Env) -> Vec<Book>` → Returns all books.
   * `count_books(env: Env) -> u32` → Returns the total number of books.

---

### Another hint, use Closure

* A closure in Rust looks like this:

  ```rust
  |param| expression
  ```

  Example:

  ```rust
  |book| book.title == title
  ```

  This means: *“Take each `book` in the list and return true if its title matches `title`.”*
* Closures are used because `iter().position()` and `into_iter().find()` need a **condition function** to decide which element matches. Instead of writing a full function, closures let you write it inline.

---

### Requirements

* Use **Soroban SDK** types like `Env`, `Symbol`, and `Vec`.
* Persist your `Library` struct in contract storage using a constant key (e.g., `"LIBRARY"`).
* Write **unit tests** for:

  * Initializing the library
  * Adding books
  * Removing a book
  * Finding a book
  * Listing all books

---

### Example Behaviors

* After initialization, the library should be empty.
* After adding `"1984"` by `"Orwell"`, `list_books` should return one book.
* After removing `"1984"`, it should no longer appear in the library.
* `find_book("Brave")` should return the `"Brave New World"` book details if it was added.

---

### Deliverables

* A Rust file containing your contract.
* A set of unit tests demonstrating that each function works as expected.
