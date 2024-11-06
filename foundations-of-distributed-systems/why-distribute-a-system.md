# Why Distribute a System?

We saw how dividing the workload among multiple chefs led to a significant reduction in meal preparation time. This illustrates one of the main benefits of distributed systems: **Scalability**

#### Scalability

Scalability in distributed systems is the ability to expand the system's capacity. In our chef example, we saw how dividing tasks among chefs allowed us to prepare all dishes concurrently. But what if the restaurant menu expanded to seven or eight dishes? Instead of overloading our current chefs or extending the time it would take to serve the food, we could simply add more chefs to handle the additional workload. This type of scaling, where we add more chefs (or nodes), is called **horizontal scaling**.

There’s also another form of scaling, known as **vertical scaling**, where instead of adding more chefs, we increase the skill level or efficiency of each chef. For example, we could upgrade a chef’s tools or workstation with say, a bigger deep fryer which allows them fry more things at a time. While vertical scaling can improve performance, horizontal scaling offers more flexibility and allows us to easily handle larger tasks by simply adding more chefs as needed.

{% hint style="info" %}
Horizontal scaling adds more nodes, whereas vertical scaling beefs up each node’s power.  For instance, upgrading a node’s RAM or processing power is vertical scaling.
{% endhint %}

#### Fault tolerance

Scalability is important, but what happens if one of our chefs fails to complete their assigned dish? Distributed systems are designed to handle such failures without disrupting the entire process. This characteristic, known as **fault tolerance**, ensures that if one chef stops cooking, the others can continue working and even take on the failed chef's dish if needed.

Returning to our [example](example.md), if a chef fails to finish their assigned meal, the system can reassign that meal to another chef. This way, the meal prep continues, and the kitchen remains resilient despite individual chef failures.

Here’s an extension of our Rust code to simulate a chef failing and reassigning their dish to another chef:

```rust
use rand::Rng;
use std::thread;
use std::time::{Duration, Instant};

// Simulated function to cook a meal, with random failure
fn cook_meal(chef_id: u32) -> Result<(), ()> {
    let mut rng = rand::thread_rng();
    let failed: bool = rng.gen(); // Randomly decide if the chef fails
    if failed {
        println!("Chef {} failed!", chef_id);
        Err(())
    } else {
        println!("Chef {} is cooking a meal...", chef_id);
        thread::sleep(Duration::from_secs(3)); // Simulate cooking time (3 seconds as minutes)
        println!("Chef {} finished cooking the meal.", chef_id);
        Ok(())
    }
}

fn main() {
    let start_time = Instant::now();
    let mut handles = vec![];

    for i in 1..=6 {
        let handle = thread::spawn(move || {
            let result = cook_meal(i);
            if result.is_err() {
                println!("Reassigning task of Chef {} to another chef.", i);
                cook_meal(i + 10).ok(); // Attempt to cook the meal again with a new ID
            }
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    let elapsed_time = start_time.elapsed();
    println!(
        "All tasks completed with fault tolerance in {:.2} minutes",
        elapsed_time.as_secs_f64()
    );
}
```

Sample Output

```bash
Chef 2 failed!
Reassigning task of Chef 2 to another chef.
Chef 12 is cooking a meal...
Chef 1 failed!
Reassigning task of Chef 1 to another chef.
Chef 11 is cooking a meal...
Chef 5 failed!
Reassigning task of Chef 5 to another chef.
Chef 15 failed!
Chef 3 is cooking a meal...
Chef 6 is cooking a meal...
Chef 4 is cooking a meal...
Chef 12 finished cooking the meal.
Chef 3 finished cooking the meal.
Chef 4 finished cooking the meal.
Chef 11 finished cooking the meal.
Chef 6 finished cooking the meal.
All tasks completed with fault tolerance in 3.01 minutes
```

#### Decentralization

In addition to scalability and fault tolerance, distributed systems can also operate in a **decentralized** manner. Decentralization means there is no central authority or single point of control. Instead, each chef operates independently and communicates directly with the others in the kitchen. This makes distributed systems more resilient because they’re not reliant on any one chef for coordination or oversight.

In our multi-course meal example, decentralization would mean that each chef works on their dish without needing permission or instruction from a head chef. Each chef can share their progress directly with the others, contributing to the final meal collectively.

Decentralization not only eliminates a single point of failure but also enhances transparency and security, especially in systems where trust and data integrity are critical—such as in blockchain or other distributed ledger systems.
