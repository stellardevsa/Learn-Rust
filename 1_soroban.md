
# Class 1: Getting Started with Soroban Smart Contracts

**Objective:** Learn how to write, build, and test your very first Soroban smart contract.

---

## Part 1: Introduction to Soroban

### What is Soroban?

* Soroban is Stellar’s **smart contract platform**.
* Smart contracts are small programs that live on the blockchain and run automatically when called.
* Example: Instead of trusting a person to send you money when you send goods, a smart contract can hold the funds and release them only when conditions are met.

### Why Rust and Soroban SDK?

* Soroban contracts are written in **Rust** because it’s safe, secure, and memory-efficient.
* Contracts compile into **WASM (WebAssembly)** so they can run on different platforms in a controlled environment.
* The **Soroban SDK** gives us special tools like `Env`, `String`, and `Vec` that work in the blockchain world.

---

## Part 2: Writing Our First Contract

### Set Up -> https://developers.stellar.org/docs/build/smart-contracts/getting-started/setup

Here’s the code we’ll work with:

```rust
#![no_std]
use soroban_sdk::{contract, contractimpl, vec, Env, String, Vec};

#[contract]
pub struct Contract;

#[contractimpl]
impl Contract {
    pub fn hello(env: Env, to: String) -> Vec<String> {
        vec![&env, String::from_str(&env, "Hello"), to]
    }
}

mod test;
```

---

### Step-by-Step Explanation

* `#![no_std]`

  * Soroban contracts **don’t use Rust’s standard library**. Instead, they run in WASM with limited resources.
  * We use `soroban_sdk` types instead of normal Rust types.

* `use soroban_sdk::{...}`

  * `contract` and `contractimpl`: macros that mark this as a blockchain contract and expose its functions.
  * `vec`: a helper macro to build vectors in Soroban.
  * `Env`: the environment — it connects the contract to the blockchain world (storage, ledger info, crypto, events).
  * `String` and `Vec`: Soroban’s own data types for strings and vectors.

* `#[contract] pub struct Contract;`

  * This defines a new contract called `Contract`.
  * It’s empty because the logic will live in the implementation block.

* `#[contractimpl] impl Contract { ... }`

  * Functions inside here become **callable contract methods**.

* `pub fn hello(env: Env, to: String) -> Vec<String>`

  * A function that takes:

    * `env` (the blockchain environment),
    * `to` (a Soroban `String`, usually a name).
  * It returns a Soroban `Vec<String>` — a list of strings.

* Inside the function:

  ```rust
  vec![&env, String::from_str(&env, "Hello"), to]
  ```

  * `vec![...]`: creates a new vector.
  * `&env`: tells Soroban where to allocate the vector.
  * `String::from_str(&env, "Hello")`: creates a Soroban string `"Hello"`.
  * `to`: the name we passed in.
  * Result: `["Hello", <name>]`.

Example: If you call `hello("Dev")`, you get back `["Hello", "Dev"]`.

---

## Part 3: Testing Our Contract

Smart contracts must be tested before deploying. Soroban makes this easy with unit tests.

Here’s the test code:

```rust
#![cfg(test)]

use super::*;
use soroban_sdk::{vec, Env, String};

#[test]
fn test() {
    let env = Env::default();
    let contract_id = env.register(Contract, ());
    let client = ContractClient::new(&env, &contract_id);

    let words = client.hello(&String::from_str(&env, "Dev"));
    assert_eq!(
        words,
        vec![
            &env,
            String::from_str(&env, "Hello"),
            String::from_str(&env, "Dev"),
        ]
    );
}
```

---

### Step-by-Step Explanation

* `#![cfg(test)]`

  * This block is only compiled when running tests, not in production.

* `use super::*;`

  * Brings our contract into the test file so we can test it.

* `let env = Env::default();`

  * Creates a **fake blockchain environment** for testing.

* `let contract_id = env.register(Contract, ());`

  * Registers (deploys) the contract inside this fake environment.
  * `()` means we don’t pass initialization arguments.

* `let client = ContractClient::new(&env, &contract_id);`

  * `ContractClient` is automatically generated when we used `#[contract]`.
  * It lets us call contract functions easily in Rust tests.

* `let words = client.hello(&String::from_str(&env, "Dev"));`

  * Calls our contract function with `"Dev"` as input.
  * Result should be `["Hello", "Dev"]`.

* `assert_eq!(words, vec![...])`

  * Checks that the contract returned exactly what we expected.
  * If it doesn’t match, the test fails.

---

### Running the Test

In your terminal, run:

```bash
stellar contract test    # Run the tests
```

If successful, you’ll see a passing test message.

### Build the contract

In your terminal, run:

```bash
stellar contract build
```

---

Deploy to the TestNet https://developers.stellar.org/docs/build/smart-contracts/getting-started/deploy-to-testnet

## Part 4: Practice & Homework

### In-Class Exercise

* Change the contract so it returns: `["Hi", <name>]`.
* Run the test and see it fail.
* Update the test to expect `"Hi"` instead of `"Hello"`.
* Run the test again and see it pass. 🎉

### Homework

1. Modify `hello` to return three words: `["Hello", <name>, "from Soroban"]`.
2. Write a test to check this new behavior.
3. Read about Soroban’s `Env` object in the docs and note 3 things it can do besides memory allocation.

---

By the end of this class, students will:

* Understand the difference between normal Rust and Soroban Rust.
* Know how to write and expose a smart contract function.
* Be able to build and test a simple contract in a local environment.

---

