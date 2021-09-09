# Lifetimes:
 A lifetime is the scope for which that reference is valid.
 ### Preventing Dangling References with Lifetimes;
 The main aim of lifetimes is to prevent dangling references, which cause a program to reference data other than the data it’s intended to reference.
 ```rs 
 fn main() {
    {
        let r;

        {
            let x = 5;
            r = &x;
        }

        println!("r: {}", r);
    }
}
```
Here an error will occure. The outer scope declares a variable named r with no initial value, and the inner scope declares a variable named x with the initial value of 5. Inside the inner scope, we attempt to set the value of r as a reference to x. Then the inner scope ends, and we attempt to print the value in r. This code won’t compile because the value r is referring to has gone out of scope before we try to use it.
### The Borrow Checker;
The Rust compiler has a borrow checker that compares scopes to determine whether all borrows are valid. 
```rs
 let r;                       // ---------+-- 'a
                              //          |
        {                     //          |
            let x = 5;        // -+-- 'b  |
            r = &x;           //  |       |
        }                     // -+       |
                              //          |
        println!("r: {}", r); //          |
```
Here, we’ve annotated the lifetime of r with 'a and the lifetime of x with 'b. As you can see, the inner 'b block is much smaller than the outer 'a lifetime block. At compile time, Rust compares the size of the two lifetimes and sees that r has a lifetime of 'a but that it refers to memory with a lifetime of 'b. The program is rejected because 'b is shorter than 'a: the subject of the reference doesn’t live as long as the reference.    
### Generic Lifetimes in Functions;
```rs 
fn main() {
    let string1 = String::from("abcd");
    let string2 = "xyz";

    let result = longest(string1.as_str(), string2);
    println!("The longest string is {}", result);
}
```
If we try to implement the longest function :
```rs
fn longest(x: &str, y: &str) -> &str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```
Instead, we get the following error that talks about lifetimes:
```
fn longest(x: &str, y: &str) -> &str {
  |           ----     ----     ^ expected named lifetime parameter
  ```
  _The help text reveals that the return type needs a generic lifetime parameter on it because __Rust can’t tell whether the reference being returned refers to x or y___

### Lifetime Annotation Syntax;
Lifetime annotations don’t change how long any of the references live. Just as functions can accept any type when the signature specifies a generic type parameter,  Lifetime annotations describe the relationships of the lifetimes of multiple references to each other without affecting the lifetimes.
```rs
&i32        // a reference
&'a i32     // a reference with an explicit lifetime
&'a mut i32 // a mutable reference with an explicit lifetime
```
One lifetime annotation by itself doesn’t have much meaning, because the annotations are meant to tell Rust how generic lifetime parameters of multiple references relate to each other. For example, let’s say we have a function with the parameter first that is a reference to an i32 with lifetime 'a. The function also has another parameter named second that is another reference to an i32 that also has the lifetime 'a. The lifetime annotations indicate that the references first and second must both live as long as that generic lifetime.

### Lifetime Annotations in Function Signatures;
```rs
fn main() {
    let string1 = String::from("abcd");
    let string2 = "xyz";

    let result = longest(string1.as_str(), string2);
    println!("The longest string is {}", result);
}

fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}

```
The function signature now tells Rust that for some lifetime 'a, the function takes two parameters, both of which are string slices that live at least as long as lifetime 'a. The function signature also tells Rust that the string slice returned from the function will live at least as long as lifetime 'a. 

When annotating lifetimes in functions, the annotations go in the function signature, not in the function body. Rust can analyze the code within the function without any help. However, when a function has references to or from code outside that function, it becomes almost impossible for Rust to figure out the lifetimes of the parameters or return values on its own. The lifetimes might be different each time the function is called. This is why we need to annotate the lifetimes manually.

### Lifetime Annotations in Struct Definitions;
```rs 
struct ImportantExcerpt<'a> {
    part: &'a str,
}

fn main() {
    let novel = String::from("Call me Ishmael. Some years ago...");
    let first_sentence = novel.split('.').next().expect("Could not find a '.'");
    let i = ImportantExcerpt {
        part: first_sentence,
    };
}
```
This struct has one field, part, that holds a string slice, which is a reference. As with generic data types, we declare the name of the generic lifetime parameter inside angle brackets after the name of the struct so we can use the lifetime parameter in the body of the struct definition. This annotation means an instance of ImportantExcerpt can’t outlive the reference it holds in its part field.

The main function here creates an instance of the ImportantExcerpt struct that holds a reference to the first sentence of the String owned by the variable novel. The data in novel exists before the ImportantExcerpt instance is created. In addition, novel doesn’t go out of scope until after the ImportantExcerpt goes out of scope, so the reference in the ImportantExcerpt instance is valid.

### Lifetime Annotations in Method Definitions;
```rs
impl<'a> ImportantExcerpt<'a> {
    fn level(&self) -> i32 {
        3
    }
}
```
The lifetime parameter declaration after impl and its use after the type name are required, but we’re not required to annotate the lifetime of the reference to self because of the first elision rule ( you have to read it from the book).

Here is an example where the third lifetime elision rule applies:
```rs

impl<'a> ImportantExcerpt<'a> {
    fn announce_and_return_part(&self, announcement: &str) -> &str {
        println!("Attention please: {}", announcement);
        self.part
    }
}
```
There are two input lifetimes, so Rust applies the first lifetime elision rule and gives both &self and announcement their own lifetimes. Then, because one of the parameters is &self, the return type gets the lifetime of &self, and all lifetimes have been accounted for.
### The Static Lifetime;
One special lifetime we need to discuss is 'static, which means that this reference can live for the entire duration of the program. All string literals have the 'static lifetime, which we can annotate as follows:


```rs
let s: &'static str = "I have a static lifetime.";
```
The text of this string is stored directly in the program’s binary, which is always available. Therefore, the lifetime of all string literals is 'static.

You might see suggestions to use the 'static lifetime in error messages. But before specifying 'static as the lifetime for a reference, think about whether the reference you have actually lives the entire lifetime of your program or not. You might consider whether you want it to live that long, even if it could. Most of the time, the problem results from attempting to create a dangling reference or a mismatch of the available lifetimes. In such cases, the solution is fixing those problems, not specifying the 'static lifetime.



## Generic Type Parameters, Trait Bounds, and Lifetimes in one place:
```rs 
use std::fmt::Display;

fn longest_with_an_announcement<'a, T>(
    x: &'a str,
    y: &'a str,
    ann: T,
) -> &'a str
where
    T: Display,
{
    println!("Announcement! {}", ann);
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```
