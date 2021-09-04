# Ownership and Borrowing:

## OwnerShip;
* Each value in Rust has a variable that’s called its owner.
* There can only be one owner at a time.
* When the owner goes out of scope, the value will be dropped.

1. 

        fn main() {
        let x = 5;
            let y = x;
            println!("{}-{}",x,y);

        }
output:

    5-5

### Scope:

* When s comes into scope, it is valid.
* It remains valid until it goes out of scope



      fn main() {
        {                      // s is not valid here, it’s not yet declared
            let s = "hello";   // s is valid from this point forward

            // do stuff with s
        }                      // this scope is now over, and s is no longer valid
    }

### **The String Type:**

String is one of the data type that is stored on the heap . It is not same as string literal (str and &str),
 in order to support a mutable, growable piece of text, we need to allocate an amount of memory on the heap, unknown at compile time, to hold the contents.

[Bht Important concept:]()

 ![Bht Zrori cheez](https://doc.rust-lang.org/book/img/trpl04-01.svg)
  This means:


*  The memory must be requested from the memory allocator at runtime.
* We need a way of returning this memory to the allocator when we’re done with our String (cleaning useable data).
  
For the second point Rust returned memory once the variable that owns it goes out of scope. Also can be drop using [drop()]() function.

2.

    fn main() {
        let s1 = String::from("hello");
        let s2 = s1;
    }


Output: 


    let s1 = String::from("hello");
    |         -- move occurs because `s1` has type `String`, which does not implement the `Copy` trait
    3 |     let s2 = s1;
    |              -- value moved here
    4 | 
    5 |     println!("{}, world!", s1);
    |                            ^^ value borrowed here after move 

because Rust don't like low runtime performance so it will give ownership of first to second in order to don't need to check both variables validity
 (if one is out of scope other also be). 

![](https://doc.rust-lang.org/book/img/trpl04-04.svg)


### Ownership and Functions:
 Passing a variable to a function will move or copy, just as assignment does.

    let s = String::from("hello");  // s comes into scope

        takes_ownership(s);             // s's value moves into the function...
                                        // ... and so is no longer valid here
If we tried to use s after the call to takes_ownership, Rust would throw a compile-time error. 

It’s possible to return multiple values using a tuple,

    fn main() {
        let s1 = String::from("hello");

        let (s2, len) = calculate_length(s1);

        println!("The length of '{}' is {}.", s2, len);
    }

    fn calculate_length(s: String) -> (String, usize) {
        let length = s.len(); // len() returns the length of a String

        (s, length)
    }

## Borrowing ;
 [Complete borrowing concepts.](https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html)


 For more details check [Hello Peter 😎](https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html, "Haqookdar")
