# Class 2: Building a Counter Smart Contract  
**Objective:** Learn how to store, update, and retrieve data in Soroban smart contracts using the blockchain environment.  

---

## Part 1: Why State Matters  

- In Class 1, our contract was **stateless** — it just returned `"Hello <name>"`.  
- Real-world contracts need to **remember things**:  
  - A balance of tokens.  
  - A record of votes.  
  - A counter that increases every time someone interacts.  

That’s where **storage** comes in. Soroban lets us store data **on-chain** using the `Env` object.  

---

## Follow this Tutorial 
-> https://developers.stellar.org/docs/build/smart-contracts/getting-started/storing-data



## Part 2: The Counter Contract (Less optomized version. Why is that?)

Here’s the code we’ll use:  

```rust
#![no_std]
use soroban_sdk::{contract, contractimpl, Env};

#[contract]
pub struct CounterContract;

#[contractimpl]
impl CounterContract {
    pub fn increment(env: Env) -> u32 {
        // 1. Get current value (default 0 if not set)
        let mut count: u32 = env.storage().persistent().get(&"count").unwrap_or(0);

        // 2. Increment it
        count += 1;

        // 3. Save it back to storage
        env.storage().persistent().set(&"count", &count);

        // 4. Return the new value
        count
    }

    pub fn get(env: Env) -> u32 {
        // Just return the stored value (or 0 if none)
        env.storage().persistent().get(&"count").unwrap_or(0)
    }
}

mod test;
```

## Step-by-Step Explanation

``pub struct CounterContract;``
Defines a new contract.

``increment(env: Env) -> u32``

- Called when we want to add 1 to our counter.

- ``env.storage().persistent()``: access persistent storage (data saved on-chain).

- ``.get(&"count")``: get the stored value under the key ``"count"``.

- ``.unwrap_or(0)``: if there’s nothing stored yet, default to ``0``.

- ``count += 1``: increment.

- ``.set(&"count", &count)``: save the new value.

- Return the updated value.

``get(env: Env) -> u32``

- Reads the current count.

- Defaults to 0 if not yet initialized.

Now our contract remembers.

## Part 3: Testing the Counter Contract

Here’s the test code:

```rust
#![cfg(test)]

use super::*;
use soroban_sdk::Env;

#[test]
fn test_counter() {
    let env = Env::default();
    let contract_id = env.register(CounterContract, ());
    let client = CounterContractClient::new(&env, &contract_id);

    // Initially, counter should be 0
    assert_eq!(client.get(), 0);

    // After first increment
    assert_eq!(client.increment(), 1);

    // After second increment
    assert_eq!(client.increment(), 2);

    // Get should now return 2
    assert_eq!(client.get(), 2);
}
```

## Step-by-Step Explanation

``assert_eq!(client.get(), 0);``
- At the start, nothing has been stored, so default is 0.

``assert_eq!(client.increment(), 1);``
- First call increases count from ``0 → 1``.

``assert_eq!(client.increment(), 2);``
- Second call increases count from ``1 → 2``.

``assert_eq!(client.get(), 2);``
- Confirm that the stored value is now 2.

Tests prove the contract behaves exactly as expected.


## Part 4: Practice & Homework

Follow the Hello World and Storing Data Tutorial on Dev Docs https://developers.stellar.org/docs/build/smart-contracts/getting-started/hello-world

## By the end of this class, you should:

- Understand storage and state in Soroban.

- Be able to create, update, and retrieve persistent data.

- Write meaningful tests for stateful contracts.


