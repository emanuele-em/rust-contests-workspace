# example-contests-workspace

Repository for writing contests in Rust with rust-competitive-helper.

More info on how to use rust-competitive-helper for contests [here](https://github.com/rust-competitive-helper/rust-competitive-helper).

# Competitive Programming Library (algo_lib)

A fast I/O and utilities library for competitive programming in Rust.

## Table of Contents
- [Input Functions](#input-functions)
- [Output Functions](#output-functions)
- [Test Types](#test-types)
- [Task Types](#task-types)

## Input Functions

### Creating Input

```rust
use algo_lib::io::input::Input;
use std::io::stdin;
// From stdin
let mut stdin = stdin();
let mut input = Input::new(&mut stdin);
// From file
let mut file = std::fs::File::open("input.txt").unwrap();
let mut input = Input::new(&mut file);
```

### Reading Methods

#### Basic Types

```rust
// Read integers
let x: i32 = input.read_int(); // reads i32
let x: i64 = input.read_long(); // reads i64
let x: usize = input.read_size(); // reads usize
let x: u32 = input.read_unsigned(); // reads u32
let x: u64 = input.read_u64(); // reads u64
let x: i128 = input.read_i128(); // reads i128
// Read string/char
let s: String = input.read_string(); // reads single word
let line: String = input.read_line(); // reads entire line
let c: char = input.read_char(); // reads single char
// Generic read (uses Readable trait)
let x: T = input.read(); // reads any type implementing Readable
```

#### Collections

```rust
// Read vectors
let n = input.read_size();
let vec: Vec<i32> = input.read_vec(n); // reads n integers
let pairs: Vec<(i32, i32)> = input.read_int_pair_vec(n); // reads n pairs
// Examples
// Input: "3\n1 2 3"
let vec = input.read_vec(3); // vec = [1, 2, 3]
// Input: "2\n1 2\n3 4"
let pairs = input.read_int_pair_vec(2); // pairs = [(1,2), (3,4)]
```

#### Input State

```rust
// Check if more input exists
if input.peek().is_some() {
// more input available
}
// Check if input is exhausted
if input.is_empty() {
// no more input
}
```

## Output Functions

### Creating Output

```rust
use algo_lib::io::output::Output;
use std::io::stdout;
// To stdout
let mut stdout = stdout();
let mut output = Output::new(&mut stdout);
// To file
let mut file = std::fs::File::create("output.txt").unwrap();
let mut output = Output::new(&mut file);
// With auto-flush
let mut output = Output::new_with_auto_flush(&mut stdout);
```


### Writing Methods

#### Basic Output

```rust
// Print without newline
output.print(42); // prints: 42
output.print("Hello"); // prints: Hello
// Print with newline
output.print_line(42); // prints: 42\n
output.print_line("Hi"); // prints: Hi\n
// Print single byte/char
output.put(b'\n'); // prints newline
```

#### Collection Output

```rust
let vec = vec![1, 2, 3];
// Print space-separated
output.print_iter(&vec); // prints: 1 2 3
// Print each element on new line
output.print_per_line(&vec); // prints: 1\n2\n3\n
// Print iterator with custom format
output.print_iter_ref(vec.iter()); // prints: 1 2 3
```

### Output Control

```rust
// Manual flush - call at end of program
output.flush();
```

## Test Types

```rust
use algo_lib::misc::test_type::TestType;
// Single test case
pub static TEST_TYPE: TestType = TestType::Single;
// Multiple test cases with count
pub static TEST_TYPE: TestType = TestType::MultiNumber;
// Multiple test cases until EOF
pub static TEST_TYPE: TestType = TestType::MultiEof;
```

## Task Types

```rust
use algo_lib::misc::test_type::TaskType;
// Regular problem
pub static TASK_TYPE: TaskType = TaskType::Classic;
// Interactive problem
pub static TASK_TYPE: TaskType = TaskType::Interactive;
```
## Complete Example
```rust
rust
use algo_lib::io::{Input, Output};
use algo_lib::misc::test_type::{TestType, TaskType};
use std::io::{stdin, stdout};
type PreCalc = ();
fn solve(input: &mut Input, out: &mut Output, test_case: usize, data: &mut PreCalc) {
    // Read array size and elements
    let n = input.read_size();
    let arr = input.read_vec(n);
    // Process array
    let sum: i32 = arr.iter().sum();
    // Output result
    out.print_line(sum);
}
pub static TEST_TYPE: TestType = TestType::MultiNumber;
pub static TASK_TYPE: TaskType = TaskType::Classic;
pub(crate) fn run(mut input: Input, mut output: Output) -> bool {
    let mut pre_calc = ();
    match TEST_TYPE {
    TestType::Single => {
        solve(&mut input, &mut output, 1, &mut pre_calc);
    }
    TestType::MultiNumber => {
        let t: usize = input.read();
        for i in 1..=t {
            solve(&mut input, &mut output, i, &mut pre_calc);
        }
    }
    TestType::MultiEof => {
        let mut i = 1;
        while input.peek().is_some() {
            solve(&mut input, &mut output, i, &mut pre_calc);
            i += 1;
        }
    }
}
output.flush();
input.is_empty()
}
```

## Tips
- Always call `output.flush()` at the end of your program
- Use appropriate integer types (`read_int()` for i32, `read_size()` for array indices)
- For multiple test cases, check TEST_TYPE and handle accordingly
- Use `read_string()` for words, `read_line()` for full lines
- When reading vectors, read the size first with `read_size()`