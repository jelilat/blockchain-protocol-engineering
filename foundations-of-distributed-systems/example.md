# Example

In this hands-on example, we will create a simulation of chefs cooking a multi-course meal using a distributed system approach in Rust.

```rust
use std::thread;
use std::time::{Duration, Instant};

// Simulated function to cook a meal (3-minute preparation time)
fn cook_meal() {
    thread::sleep(Duration::from_secs(3)); // Simulate cooking time with a 3-second delay (representing 3 minutes)
}

fn main() {
    // Simulate cooking all meals with a single chef
    println!("Cooking all meals with a single chef...");
    let start = Instant::now();
    for _ in 0..6 {
        cook_meal();
    }
    let single_chef_time = start.elapsed();
    println!(
        "Single-chef preparation time: {:.2?} minutes",
        single_chef_time.as_secs_f64()
    );

    // Simulate cooking meals in a distributed system with 6 chefs
    println!("\nCooking meals in a distributed system with 6 chefs...");
    let start = Instant::now();
    let mut handles = vec![];

    for _ in 0..6 {
        // Spawn a thread for each chef, simulating each chef cooking one meal concurrently
        let handle = thread::spawn(|| {
            cook_meal();
        });
        handles.push(handle);
    }

    // Wait for all chefs to finish
    for handle in handles {
        handle.join().unwrap();
    }

    let multi_chef_time = start.elapsed();
    println!(
        "Multi-chef (6 chefs) preparation time: {:.2?} minutes",
        multi_chef_time.as_secs_f64()
    );

    // Summary
    println!("\nSummary:");
    println!(
        "Single-chef time: {:.2?} minutes",
        single_chef_time.as_secs_f64()
    );
    println!(
        "Multi-chef time: {:.2?} minutes",
        multi_chef_time.as_secs_f64()
    );
    println!(
        "Time saved with distribution: {:.2?} minutes",
        (single_chef_time - multi_chef_time).as_secs_f64()
    );
}
```

Running the code above should give you a result similar to the following

```bash
Cooking all meals with a single chef...
Single-chef preparation time: 18.02 minutes

Cooking meals in a distributed system with 6 chefs...
Multi-chef (6 chefs) preparation time: 3.00 minutes

Summary:
Single-chef time: 18.02 minutes
Multi-chef time: 3.00 minutes
Time saved with distribution: 15.01 minutes
```
