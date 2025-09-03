# Final Project: Bookstore Management App

## Project Goal  

In this project, you will build a **Bookstore Management Application** in Rust.  
The goal is to **simulate a very basic digital bookstore system** where you can add books, view all available books, and search for specific books by their ID.  

You are **not** building a full app with user input or databases — instead, you’ll practice the **core Rust concepts** you’ve learned so far:  

- **Structs & Methods** → Represent books with custom data types and behaviors.  
- **Functions** → Break your code into reusable pieces (e.g., adding, listing, searching books).  
- **Collections (`Vec`)** → Store multiple books inside a single list.  
- **Control Flow (if/else, match)** → Validate book data and handle search results.  
- **Modules** → Keep your project organized with multiple files.  

At the end of this project, you’ll have a working program that:  

1. **Creates a list of books** (the bookstore inventory).  
2. **Adds new books** to the list using a constructor.  
3. **Lists all books** in the store with nicely formatted output.  
4. **Searches for a book by ID** and prints whether it exists or not.  
5. (Optional) **Applies a discount** to a book to simulate promotions.  

This is similar to how **real-world inventory systems** work — just on a smaller scale.

---

## Requirements  

### 1. Create a `Book` struct
- Fields:  
  - `id: u32`  
  - `title: String`  
  - `author: String`  
  - `price: f64`  

---

### 2. Implement methods for `Book`
- `new(...)` → constructor that creates a new book.  
  - Use **if/else** to validate:  
    - If `title` is empty → set it to `"Untitled"`.  
    - If `price < 0` → set it to `0`.  
- `display(&self)` → prints details of the book.  

---

### 3. Create functions to manage the bookstore
- `add_book(list: &mut Vec<Book>, book: Book)` → adds a book to the collection.  
- `list_books(list: &Vec<Book>)` → displays all books.  
- `find_book(list: &Vec<Book>, id: u32)` → search by ID using **match**.  
  - If found → show the book.  
  - If not found → print `"Book not found"`.  

---

### 4. In `main.rs`
- Create a `Vec<Book>` to store books.  
- Add at least **3 books** using the constructor.  
- Call the functions in this order:  
  1. Add books  
  2. List all books  
  3. Search for an existing book  
  4. Search for a non-existing book  

---

### 5. Extra Challenge (Optional)
- Add a method `apply_discount(&mut self, percent: f64)` that reduces the price by a percentage (e.g., 10%).  
- Call it on one book before displaying it.  

---

## Example Output

```csharp
Adding books...

Listing all books:
ID: 1 | 'Rust for Beginners' by Alice | $29.99
ID: 2 | 'Mastering Rust' by Bob | $39.99
ID: 3 | 'Rust Cookbook' by Carol | $24.99

Searching for book with ID 2...
ID: 2 | 'Mastering Rust' by Bob | $39.99

Searching for book with ID 5...
Book not found.
```


---

## Concepts Practiced
By the end, students will have:  
- Practiced **structs & methods**  
- Used **functions**  
- Organized code with **modules**  
- Managed a **collection (`Vec`)**  
- Applied **control flow & decision making**  


