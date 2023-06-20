# Example

```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func solveEquation(i int, wg *sync.WaitGroup) {
    defer wg.Done()
    fmt.Printf("Solving equation %v...\n", i)
    // Simulate the time it takes to solve the equation.
    time.Sleep(3 * time.Second)
    fmt.Printf("Finished solving equation %v!\n", i)
}

func main() {
    // Serial Processing
    start := time.Now()
    for i := 1; i <= 6; i++ {
        fmt.Printf("Solving equation %v...\n", i)
        // Simulate the time it takes to solve the equation.
        time.Sleep(3 * time.Second)
        fmt.Printf("Finished solving equation %v!\n", i)
    }
    elapsed := time.Since(start)
    fmt.Printf("Total time taken for serial processing: %s\n", elapsed)

    // Parallel Processing
    start = time.Now()
    var wg sync.WaitGroup
    for i := 1; i <= 6; i++ {
        wg.Add(1)
        go solveEquation(i, &wg) // Start a goroutine to solve each equation concurrently.
    }
    wg.Wait() // Wait for all equations to be solved.
    elapsed = time.Since(start)
    fmt.Printf("Total time taken for parallel processing: %s\n", elapsed)
}

```
