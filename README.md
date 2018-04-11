## Welcome to Rust

Rust is a systems programming language that runs blazingly fast, prevents segfaults, and guarantees thread safety.

###  hahaha

hehehehehhe


### Ownership in Rust
Rust features strict ownership rules which makes it a very safe language. It is difficult to encounter null pointers, dangling pointers, and other segfaults. 

The Three Rules of Ownership

- Each value has a variable that’s called its owner.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.

```
  let s = String::from("hello");
  S is the owner of the string "hello"
  ```  
  
Here's an example code that will not compile:

  ```
  fn main() {
    let foo = String::from("cool String");
    let bar = foo;
    println!("{}", foo);
}
  
```
After we assign foo as the owner of "cool String" and then assign bar as the owner of foo, ownership of "cool String" is moved from bar to foo. Now, foo is not longer a valid reference and cannot be used.

### Borrowing in Rust
Sometimes there are situations where you want to use a value without taking ownership of it. That's called borrowing.

Rust's rules for borrowng:

1. At any given time, you can have either but not both of:
 - One mutable reference.
 - Any number of immutable references.
2. References must always be valid.

To borrow a value, we use an ampersand:
```
fn fxn() {
    let s = String::from("theString");
    doStuff(&s);
}

//s is the owner of the string and the fxn doStuff will borrow s

fn doStuff(some_string: &String) {
    println!("{}",some_string);
}

fn main(){ 
	fxn()
}

//This will print s
```

#### There are also mutable and immutable references

THIS DON'T WORK! You can not change a value that you're borrowing because you should always return things in pristine condition. 
```
fn main() {
    let s = String::from("hello");

    change(&s);
}

fn change(some_string: &String) {
    some_string.push_str(", world");
}
```

What we have to do to fix this is make "s" a mutable reference using the mut keyword:

```
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

Limits on mutable and immutable references:

```
let mut s = String::from("hello");
let r1 = &s; //nice
let r2 = &s; //cool
let r3 = &mut s; //NO WAY

//you can have as many immutable references as you want, but you can not have them along with a mutable reference.
```

```
let mut s = String::from("hello");
let r1 = &mut s; //good
let r2 = &mut s; //not good

//you can only have one mutable reference
```

### Threads
Threads can be used to run code simutaneously. However, THREADS ARE NOT SAFE! Often, they encounter problems like race conditions, deadlocks, and other bugs. Rust has features that make threads safer.

Here's a way we can create a new thread using ```thread::spawn```.
```
use std::thread;
use std::time::Duration;

fn main() {
    thread::spawn(|| {
       for i in 1..10 {
            println!("result {} from the spawned thread!", i);
            thread::sleep(Duration::from_millis(1));
       }
    });
    for i in 1..5 {
       println!("result {} from the main thread!", i);
       thread::sleep(Duration::from_millis(1));
  }
}
```
The new thread prints one thing while the main thread prints something else. In this case, the new thread will stop running when the main thread does. The results would look similar to:

```
result 1 from the main thread!
result 1 from the spawned thread!
result 2 from the main thread!
result 2 from the spawned thread!
result 3 from the main thread!
result 3 from the spawned thread!
result 4 from the main thread!
result 4 from the spawned thread!
```
Using ```thread::sleep``` forces the thread to stop running for a specified amount of time so the other one can run. It's not always guaranteed that the threads will alternate--that depends on the way your operating system schedules the threads. 

There is no guarantee about the order that the threads run or even if they run at all. In the above example, the spawned thread doesn't get to finish running. That's okay. Rust has something called a join handle that fixes this:

```
use std::thread;
use std::time::Duration;

fn main() {
    let handle = thread::spawn(|| {
       for i in 1..10 {
            println!("result {} from the spawned thread!", i);
            thread::sleep(Duration::from_millis(1));
       }
    });
    
    handle.join(); //here's a join handle!! since it's called after the spawned thread, that's the thread it represents.
    
    for i in 1..5 {
       println!("result {} from the main thread!", i);
       thread::sleep(Duration::from_millis(1));
  }
}
```

Calling a join handle blocks a thread from running until the thread that it represents has finished running. The results would look like:

```
result 1 from the spawned thread!
result 2 from the spawned thread!
result 3 from the spawned thread!
result 4 from the spawned thread!
result 5 from the spawned thread!
result 6 from the spawned thread!
result 7 from the spawned thread!
result 8 from the spawned thread!
result 9 from the spawned thread!
result 1 from the main thread!
result 2 from the main thread!
result 3 from the main thread!
result 4 from the main thread!
```
