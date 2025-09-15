# Soroban SDK Macros Guide

**View the docs** -> https://docs.rs/soroban-sdk/latest/soroban_sdk/

Soroban macros are special functions that help you write smart contracts more easily. Think of them as shortcuts that generate code for you automatically. Let's explore the two types of macros available.

## Function-like Macros
These macros work like functions - you call them with parameters and they return something or perform an action.

### **Data Creation Macros**

**`bytes!()`** - Create binary data
```rust
let my_data = bytes!(b"Hello World");  // From byte string
let hex_data = bytes!(0x48656c6c6f);   // From hex
```
*Think of this as converting text or numbers into computer-readable binary format.*

**`bytesn!()`** - Create fixed-size binary data
```rust
let fixed_data = bytesn!(32, b"This is exactly 32 bytes long!!"); 
```
*Like `bytes!()` but guarantees a specific size - useful for cryptographic keys or hashes.*

**`map!()`** - Create key-value storage
```rust
let scores = map! {
    "alice" => 100,
    "bob" => 85,
    "charlie" => 92
};
```
*Creates a dictionary where you can look up values by their keys.*

**`vec!()`** - Create lists
```rust
let my_list = vec![1, 2, 3, 4, 5];
let names = vec!["Alice", "Bob", "Charlie"];
```
*Creates an ordered list of items that can grow or shrink.*

**`symbol_short!()`** - Create short text identifiers
```rust
let token_symbol = symbol_short!("USD");
let action = symbol_short!("transfer");
```
*Creates efficient short text labels (max 32 characters) used as identifiers.*

### **Contract Management Macros**

**`contractfile!()`** - Import contract code
```rust
const MY_CONTRACT: &[u8] = contractfile!("path/to/contract.wasm");
```
*Loads a compiled contract file so you can deploy or reference it.*

**`contractimport!()`** - Import contract with full setup
```rust
contractimport!(file = "token_contract.wasm");
// This generates client code, types, and imports the contract
```
*Does everything `contractfile!()` does, plus creates helper code to interact with the contract.*

**`contractmeta!()`** - Add contract metadata
```rust
contractmeta!(
    key = "version",
    val = "1.0.0"
);
```
*Embeds information about your contract (like version, description) that others can read.*

### **Development & Debugging Macros**

**`log!()`** - Debug logging
```rust
log!("User balance: {}", user_balance);
log!("Contract initialized successfully");
```
*Prints debug messages - only works during testing, not in production.*

**`assert_in_contract!()`** - Verify contract context
```rust
assert_in_contract!(); // Ensures code is running inside a contract
```
*Safety check to make sure your code is running in the right environment.*

**`assert_with_error!()`** - Conditional panic with custom error
```rust
assert_with_error!(balance > 0, MyError::InsufficientFunds);
```
*Stops execution with a specific error message if a condition isn't met.*

**`panic_with_error!()`** - Immediate panic with custom error
```rust
panic_with_error!(MyError::Unauthorized);
```
*Immediately stops execution and returns a specific error.*

## Attribute Macros
These macros are applied to types (structs, enums) using `#[macro_name]` syntax. They automatically generate additional code for your types.

### **Core Contract Macros**

**`#[contract]`** - Mark the main contract type
```rust
#[contract]
pub struct MyToken;

impl MyToken {
    pub fn transfer(from: Address, to: Address, amount: i128) {
        // Contract function
    }
}
```
*Tells Soroban "this is my main contract" - all public functions become contract methods.*

**`#[contractimpl]`** - Export contract functions
```rust
#[contractimpl]
impl MyToken {
    pub fn get_balance(user: Address) -> i128 {
        // This becomes a callable contract function
    }
}
```
*Makes your contract functions callable from outside the contract.*

### **Helper Generation Macros**

**`#[contractclient]`** - Generate client code
```rust
#[contractclient(name = "MyTokenClient")]
pub trait MyTokenTrait {
    fn transfer(from: Address, to: Address, amount: i128);
}
```
*Automatically creates code that other contracts can use to call your contract.*

**`#[contractargs]`** - Generate function argument helpers
```rust
#[contractargs]
pub struct TransferArgs {
    pub from: Address,
    pub to: Address, 
    pub amount: i128,
}
```
*Creates helper code for organizing function parameters.*

### **Data Type Macros**

**`#[contracttype]`** - Make types contract-compatible
```rust
#[contracttype]
pub struct User {
    pub name: String,
    pub balance: i128,
}
```
*Allows your custom types to be stored in contract storage and passed between contracts.*

**`#[contracterror]`** - Create custom error types
```rust
#[contracterror]
pub enum TokenError {
    InsufficientBalance = 1,
    Unauthorized = 2,
    InvalidAmount = 3,
}
```
*Defines specific error codes your contract can return when something goes wrong.*

**`#[contractevent]`** - Create event types
```rust
#[contractevent]
pub struct TransferEvent {
    pub from: Address,
    pub to: Address,
    pub amount: i128,
}
```
*Defines events that your contract can publish - like notifications that others can listen to.*

## Quick Summary

- **Function-like macros** (`macro!()`) are called like functions to create data or perform actions
- **Attribute macros** (`#[macro]`) are applied to types to automatically generate additional functionality
- Use these macros to reduce boilerplate code and ensure your contract works properly with the Soroban environment
- Most beginners will start with `#[contract]`, `#[contractimpl]`, and data creation macros like `vec!()` and `map!()`

Remember: These macros are code generators that save you from writing repetitive code manually!

## Core Soroban Data Types (Structs)

Understanding Soroban's built-in data types is crucial for writing effective smart contracts. These types are optimized for blockchain storage and inter-contract communication.

### **Identification & Addressing**

**`Address`** - Universal identifier for accounts and contracts
```rust
let user_addr: Address = Address::from_string("GCZJ...XYZ");
let contract_addr: Address = env.current_contract_address();
```
*Think of this as a postal address - it uniquely identifies where to send tokens or call functions. Works for both user accounts and smart contracts.*


### **Binary Data Types**

**`Bytes`** - Flexible binary data (growable)
```rust
let mut data = Bytes::new(&env);
data.append(&Bytes::from_slice(&env, b"Hello"));
data.append(&Bytes::from_slice(&env, b" World"));
```
*Like a flexible container for any binary data - can grow and shrink as needed. Good for storing files, encrypted data, or variable-length content.*

**`BytesN<N>`** - Fixed-size binary data
```rust
let hash: BytesN<32> = BytesN::from_array(&env, &[1u8; 32]);
let key: BytesN<64> = cryptographic_key; 
```
*Like a rigid box that always holds exactly N bytes. Perfect for cryptographic hashes, keys, or any data that must be a specific size.*

### **Text & Symbols**

**`String`** - Flexible text storage
```rust
let mut message = String::from_str(&env, "Hello");
message.append(&String::from_str(&env, " World"));
```
*Standard text that can grow and change. Use for user messages, descriptions, or any variable-length text.*

**`Symbol`** - Efficient short identifiers
```rust
let token_symbol = Symbol::new(&env, "USD");
let function_name = Symbol::new(&env, "transfer");
```
*Super-efficient short text (max 32 chars) with limited characters (letters, numbers, underscore). Perfect for token symbols, function names, or keys.*

### **Collections**

**`Vec<T>`** - Ordered list
```rust
let mut numbers: Vec<i32> = Vec::new(&env);
numbers.push_back(10);
numbers.push_back(20);
let first = numbers.get(0); // Returns 10
```
*An ordered list where you access items by position (0, 1, 2...). Like a numbered list where you can add, remove, and rearrange items.*

**`Map<K, V>`** - Key-value storage
```rust
let mut balances: Map<Address, i128> = Map::new(&env);
balances.set(user_addr, 1000);
let balance = balances.get(user_addr); // Returns 1000
```
*A dictionary where you look up values using keys. Perfect for user balances, settings, or any "name → value" relationships.*

### **Large Numbers**

**`I256`** - Very large signed integers
```rust
let big_positive = I256::from_i32(&env, 1000000);
let big_negative = I256::from_i32(&env, -500000);
let result = big_positive + big_negative;
```
*Handles incredibly large positive and negative numbers (way bigger than regular integers). Essential for high-value financial calculations.*

**`U256`** - Very large unsigned integers  
```rust
let huge_supply = U256::from_u64(&env, 1_000_000_000_000u64);
let total_value = huge_supply * U256::from_u32(&env, 100);
```
*Like I256 but only positive numbers. Perfect for token supplies, large quantities, or any calculation that can't be negative.*


### **System & Utility Types**

**`Env`** - Contract's connection to the blockchain
```rust
pub fn my_function(env: Env) {
    let current_time = env.ledger().timestamp();
    let caller = env.invoker();
    env.storage().persistent().set(&key, &value);
}
```
*Your contract's "control panel" - provides access to storage, time, caller info, and all blockchain functions.*

**`Val`** - Universal data container
```rust
let val: Val = 42i32.into_val(&env);
let string_val: Val = "hello".into_val(&env);
```
*The "universal translator" - any Soroban type can be converted to/from Val. Used internally for storage and contract communication.*

**`ConversionError`** - Type conversion failures
```rust
match try_convert_to_address(some_val) {
    Ok(addr) => // Use the address
    Err(ConversionError) => // Handle the error
}
```
*Error type when converting between types fails (like trying to convert "abc" to a number). Helps you handle invalid data gracefully.*

## Type Selection Guide

**For storing user data:** Use `Address`, `String`, `I256`/`U256` for amounts  
**For collections:** `Map` for lookups, `Vec` for ordered lists  
**For identifiers:** `Symbol` for short IDs, `String` for longer names  
**For binary data:** `BytesN` for fixed sizes (hashes), `Bytes` for flexible data  
**For math:** `I256`/`U256` for financial calculations, regular `i32`/`u64` for simple counts

Remember: Choose the right type for your use case - it affects both performance and storage costs!
