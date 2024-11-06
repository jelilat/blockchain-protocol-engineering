# Hands-On: Simulating a Distributed Ledger in Rust

Let us dive into Distributed Ledger Technology (DLT) with a project that feels a bit like a real DLT in action. Using our book club example, we’ll see how each member maintains their own ledger and agrees on the state of book transactions. With this approach, we simulate the transparency and trust of DLT without a central authority.

<pre class="language-rust"><code class="lang-rust">```rust
<strong>use std::collections::HashMap;
</strong>use std::sync::{Arc, Mutex};
use std::thread;
use std::time::{SystemTime, UNIX_EPOCH};

// Represents a book in the book club
// Each book has a title and an owner
#[derive(Debug, Clone)]
struct Book {
    title: String,
    owner: String,
}

// Defines the possible actions that can be performed with a book
// BORROW: When a member takes a book
// RETURN: When a member returns a book
#[derive(Debug, Clone, PartialEq)]
enum Action {
    BORROW,
    RETURN,
}

// Represents a single transaction in the distributed ledger
// Each transaction records:
// - When it happened (timestamp)
// - What book was involved
// - Who borrowed/returned it
// - What action was taken
// - Any optional notes about the transaction
#[derive(Debug, Clone)]
struct BookTransaction {
    timestamp: u64,
    book: Book,
    borrower: String,
    action: Action,
    notes: Option&#x3C;String>,
}

// The main ledger structure that each member maintains
// This is the "distributed" part - each member has their own copy
#[derive(Debug, Clone)]
struct BookClubLedger {
    member_name: String,
    entries: Vec&#x3C;BookTransaction>, // Confirmed transactions
    pending_transactions: Vec&#x3C;BookTransaction>, // Transactions awaiting validation
}

impl BookClubLedger {
    // Creates a new ledger for a member
    fn new(member_name: String) -> Self {
        BookClubLedger {
            member_name,
            entries: vec![],
            pending_transactions: vec![],
        }
    }

    /// Proposes a new transaction to the network
    /// This is the first step in the DLT process:
    /// 1. A member initiates a transaction
    /// 2. The transaction is timestamped
    /// 3. It's added to pending transactions
    /// 4. Other members will later validate it
    fn propose_transaction(
        &#x26;mut self,
        book: Book,
        borrower: String,
        action: Action,
        notes: Option&#x3C;String>,
    ) -> BookTransaction {
        let transaction = BookTransaction {
            // Create a unique timestamp for the transaction
            timestamp: SystemTime::now()
                .duration_since(UNIX_EPOCH)
                .unwrap()
                .as_secs(),
            book,
            borrower,
            action,
            notes,
        };
        self.pending_transactions.push(transaction.clone());
        println!("{} recorded: {:?}", self.member_name, transaction);
        transaction
    }

    /// Validates a transaction and adds it to the ledger if valid
    /// This implements the consensus mechanism:
    /// - Checks if the book is available for borrowing
    /// - Ensures the transaction follows the rules
    /// - Adds valid transactions to the permanent record
    fn validate_and_add_transaction(&#x26;mut self, transaction: BookTransaction) -> bool {
        // For BORROW actions, we need to verify the book isn't already borrowed
        if transaction.action == Action::BORROW {
            let last_transaction = self
                .entries
                .iter()
                .rev()
                .find(|t| t.book.title == transaction.book.title);

            if let Some(last) = last_transaction {
                if last.action == Action::BORROW {
                    println!(
                        "{} rejected transaction: Book already borrowed",
                        self.member_name
                    );
                    return false;
                }
            }
        }

        // If validation passes, add to the permanent record
        self.entries.push(transaction);
        true
    }
}

fn main() {
    // Shared state for pending transactions
    // In a real DLT, this would be replaced by network communication
    let shared_pending = Arc::new(Mutex::new(Vec::new()));
    let mut members = HashMap::new();
    let mut handles = vec![];

    // Initialize the book club members
    // Each member gets their own copy of the ledger
    let member_names = vec!["Alice", "Bob", "Charlie"];
    for name in member_names {
        let ledger = Arc::new(Mutex::new(BookClubLedger::new(name.to_string())));
        members.insert(name.to_string(), Arc::clone(&#x26;ledger));
    }

    // Define the books available in the book club
    let books = vec![
        Book {
            title: "1984".to_string(),
            owner: "Bob".to_string(),
        },
        Book {
            title: "The Hobbit".to_string(),
            owner: "Alice".to_string(),
        },
        Book {
            title: "Dune".to_string(),
            owner: "Charlie".to_string(),
        },
    ];

    // Define a series of transactions to simulate
    let transactions = vec![
        (
            "Alice",
            books[0].clone(),
            Action::BORROW,
            Some("Excited to read this classic!".to_string()),
        ),
        ("Charlie", books[1].clone(), Action::BORROW, None),
        (
            "Alice",
            books[0].clone(),
            Action::RETURN,
            Some("Great read!".to_string()),
        ),
    ];

    // Process each transaction in a separate thread
    // This simulates distributed processing of transactions
    for (borrower, book, action, notes) in transactions {
        let ledger = members.get(book.owner.as_str()).unwrap().clone();
        let shared_pending = Arc::clone(&#x26;shared_pending);
        let members = members.clone();

        let handle = thread::spawn(move || {
            // Step 1: Record the transaction in the owner's ledger
            let transaction = {
                let mut ledger = ledger.lock().unwrap();
                ledger.propose_transaction(book, borrower.to_string(), action, notes)
            };

            // Step 2: Broadcast the transaction to all members
            // In a real DLT, this would be network broadcast
            {
                let mut pending = shared_pending.lock().unwrap();
                pending.push(transaction);
            }

            // Step 3: Each member validates and updates their ledger
            // This is the consensus mechanism in action
            let pending = shared_pending.lock().unwrap();
            for transaction in pending.iter() {
                for (_, member_ledger) in members.iter() {
                    let mut member_ledger = member_ledger.lock().unwrap();
                    if member_ledger.validate_and_add_transaction(transaction.clone()) {
                        println!("{} validated the transaction", member_ledger.member_name);
                    }
                }
            }
        });
        handles.push(handle);
    }

    // Wait for all transactions to complete
    for handle in handles {
        handle.join().unwrap();
    }

    // Display the final state of all ledgers
    // This demonstrates that all members have the same transaction history
    println!("\nFinal Ledger States:");
    for (name, ledger) in members.iter() {
        let ledger = ledger.lock().unwrap();
        println!("\n{}'s Records:", name);
        for entry in &#x26;ledger.entries {
            println!(
                "- {} {:?} '{}' {} {} at timestamp {}{}",
                entry.borrower,
                entry.action,
                entry.book.title,
                if entry.action == Action::BORROW {
                    "from"
                } else {
                    "to"
                },
                entry.book.owner,
                entry.timestamp,
                entry
                    .notes
                    .as_ref()
                    .map_or(String::new(), |n| format!(" (Note: {})", n))
            );
        }
    }
}

```
</code></pre>

The output

```bash
Bob recorded: BookTransaction { timestamp: 1730876458, book: Book { title: "1984", owner: "Bob" }, borrower: "Alice", action: BORROW, notes: Some("Excited to read this classic!") }
Alice recorded: BookTransaction { timestamp: 1730876458, book: Book { title: "The Hobbit", owner: "Alice" }, borrower: "Charlie", action: BORROW, notes: None }
Bob recorded: BookTransaction { timestamp: 1730876458, book: Book { title: "1984", owner: "Bob" }, borrower: "Alice", action: RETURN, notes: Some("Great read!") }
Alice validated the transaction
Bob validated the transaction
Charlie validated the transaction
Alice rejected transaction: Book already borrowed
Bob rejected transaction: Book already borrowed
Charlie rejected transaction: Book already borrowed
Alice validated the transaction
Bob validated the transaction
Charlie validated the transaction
Alice rejected transaction: Book already borrowed
Bob rejected transaction: Book already borrowed
Charlie rejected transaction: Book already borrowed
Alice rejected transaction: Book already borrowed
Bob rejected transaction: Book already borrowed
Charlie rejected transaction: Book already borrowed
Alice validated the transaction
Bob validated the transaction
Charlie validated the transaction

Final Ledger States:

Alice's Records:
- Alice BORROW '1984' from Bob at timestamp 1730876458 (Note: Excited to read this classic!)
- Charlie BORROW 'The Hobbit' from Alice at timestamp 1730876458
- Alice RETURN '1984' to Bob at timestamp 1730876458 (Note: Great read!)

Bob's Records:
- Alice BORROW '1984' from Bob at timestamp 1730876458 (Note: Excited to read this classic!)
- Charlie BORROW 'The Hobbit' from Alice at timestamp 1730876458
- Alice RETURN '1984' to Bob at timestamp 1730876458 (Note: Great read!)

Charlie's Records:
- Alice BORROW '1984' from Bob at timestamp 1730876458 (Note: Excited to read this classic!)
- Charlie BORROW 'The Hobbit' from Alice at timestamp 1730876458
- Alice RETURN '1984' to Bob at timestamp 1730876458 (Note: Great read!)
```

In this version of the book club ledger:

* Each member maintains a ledger of book transactions.
* Transactions can be proposed (like borrowing or returning a book).
* Each transaction is validated across members, creating a “consensus” before being added to the final ledger.

This collaborative approach ensures that each member’s records are consistent, transparent, and reflect all agreed-upon transactions.

In the next section, we’ll delve deeper into **Blockchain**, a specialized form of DLT that not only achieves decentralization and transparency but also ensures **immutability** and **consensus** through unique cryptographic and algorithmic structures.
