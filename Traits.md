# Traits:
A trait tells the Rust compiler about functionality a particular type has and can share with other types. 
fpr example;

we’ve defined the desired behavior using the Summary trait, we can implement it on the types in our media aggregator. 
```rs
pub trait Summary {
    fn summarize(&self) -> String;
}

pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}

pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}

impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}
```
Implementing a trait on a type is similar to implementing regular methods. The difference is that after impl, we put the trait name that we want to implement, then use the for keyword, and then specify the name of the type we want to implement the trait for. Within the impl block, we put the method signatures that the trait definition has defined. Instead of adding a semicolon after each signature, we use curly brackets and fill in the method body with the specific behavior that we want the methods of the trait to have for the particular type.

### Default Implementations;
 As we implement the trait on a particular type, we can keep or override each method’s default behavior.
```rs
 pub trait Summary {
    fn summarize(&self) -> String {
        String::from("(Read more...)")
    }
}

pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summary for NewsArticle {}

pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}

impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}
```

### Traits as Parameters;
We can define a notify function that calls the summarize method on its item parameter, which is of some type that implements the Summary trait. 
```rs
pub fn notify(item: &impl Summary) {
    println!("Breaking news! {}", item.summarize());
}
```
---

This parameter accepts any type that implements the specified trait. In the body of notify, we can call any methods on item that come from the Summary trait, such as summarize.

### Trait Bound Syntax;
```rs
pub fn notify<T: Summary>(item: &T) {
    println!("Breaking news! {}", item.summarize());
}
```

The impl Trait syntax is convenient and makes for more concise code in simple cases. The trait bound syntax can express more complexity in other cases. For example, we can have two parameters that implement Summary. Using the impl Trait syntax looks like this:
```rs
pub fn notify(item1: &impl Summary, item2: &impl Summary) {
```

 If we wanted to force both parameters to have the same type, that’s only possible to express using a trait bound, like this:
 ```rs
 pub fn notify<T: Summary>(item1: &T, item2: &T) {
```

### Specifying Multiple Trait Bounds with the + Syntax;
We can also specify more than one trait bound  using the + syntax.
```rs
//with parameters.
pub fn notify(item: &(impl Summary + Display)) {

//with Traits bounds.
pub fn notify<T: Summary + Display>(item: &T) {

``` 
### Returning Types that Implement Traits;

```rs 
fn returns_summarizable() -> impl Summary {
    Tweet {
        username: String::from("horse_ebooks"),
        content: String::from(
            "of course, as you probably already know, people",
        ),
        reply: false,
        retweet: false,
    }
}
```
By using impl Summary for the return type, we specify that the returns_summarizable function returns some type that implements the Summary trait without naming the concrete type. 

**Important thing :** you can only use impl Trait if you’re returning a single type. For example, this code that returns either a NewsArticle or a Tweet with the return type specified as impl Sum

```rs 
fn returns_summarizable(switch: bool) -> impl Summary {
    if switch {
        NewsArticle {
            headline: String::from(
                "Penguins win the Stanley Cup Championship!",
            ),
            location: String::from("Pittsburgh, PA, USA"),
            author: String::from("Iceburgh"),
            content: String::from(
                "The Pittsburgh Penguins once again are the best \
                 hockey team in the NHL.",
            ),
        }
    } else {
        Tweet {
            username: String::from("horse_ebooks"),
            content: String::from(
                "of course, as you probably already know, people",
            ),
            reply: false,
            retweet: false,
        }
    }
}
```
Returning either a NewsArticle or a Tweet isn’t allowed due to restrictions around how the impl Trait syntax is implemented in the compiler.

### Using Trait Bounds to Conditionally Implement Methods;

```rs
use std::fmt::Display;

struct Pair<T> {
    x: T,
    y: T,
}

impl<T> Pair<T> {
    fn new(x: T, y: T) -> Self {
        Self { x, y }
    }
}

impl<T: Display + PartialOrd> Pair<T> {
    fn cmp_display(&self) {
        if self.x >= self.y {
            println!("The largest member is x = {}", self.x);
        } else {
            println!("The largest member is y = {}", self.y);
        }
    }
}
```
we can implement methods conditionally for types that implement the specified traits. 