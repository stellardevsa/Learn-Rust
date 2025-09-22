# Homework Assignment

You’ve been given the following smart contract that keeps track of a simple counter:

```rust
#![no_std]
use soroban_sdk::{contract, contractimpl, symbol_short, Env, Symbol};

const COUNTER: Symbol = symbol_short!("Counter");

#[contract]
pub struct IncrementContract;

#[contractimpl]
impl IncrementContract {
    pub fn increment(env: Env) -> i32 {
        let mut count: i32 = env.storage().instance().get(&COUNTER).unwrap_or(0);

        count += 1;

        env.storage().instance().set(&COUNTER, &count);
        env.storage().instance().extend_ttl(50, 100);

        count
    }
}
```

And here is the test code for it:

```rust
#![cfg(test)]
use super::*;
use soroban_sdk::Env;

#[test]
fn test_increment() {
    let env = Env::default();
    let contract_id = env.register(IncrementContract, ());
    let client = IncrementContractClient::new(&env, &contract_id);

    assert_eq!(client.increment(), 1);
    assert_eq!(client.increment(), 2);
    assert_eq!(client.increment(), 3);
}
```
Your Tasks

Add a ``get_current_value`` function that returns the current value of the counter.

Add a ``decrement`` function that decreases the counter by 1.

Add a ``reset`` function that sets the counter back to 0.

Modify the test code to make sure:

- ``get_current_value`` returns the correct counter value.
- ``decrement`` correctly reduces the counter.
- ``reset`` sets the counter back to 0

*Hint: Use the same storage logic shown in increment when writing your new functions.*
