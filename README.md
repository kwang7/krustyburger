## Welcome to Rust

Rust is blahb blahb albhalbh

###  hahaha

hehehehehhe


### Ownership in Rust

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


##### Ss


