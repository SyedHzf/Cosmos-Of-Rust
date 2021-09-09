# Vector:
Note:"vector data Store in heap memory so we have to use & in loop , functions or slicing."

Vectors allow you to store more than one value in a single data structure that puts all the values next to each other in memory. Vectors can only store values of the same type. They are useful when you have a list of items, such as the lines of text in a file or the prices of items in a shopping cart.

To create a new, empty vector,

```rust 
fn main() {
    let v: Vec<i32> = Vec::new();
    }
```

Note that we added a type annotation here.when a specific vector holds a specific type, the type is specified within angle brackets.

It’s more common to create a Vec<T> that has initial values, and Rust provides the vec! macro for convenience.

 ```rs
    fn main() {
        let v = vec![1, 2, 3];
    }
```

### Updating a Vector;
To create a vector and then add elements to it, we can use the push method
but vector must be mutable.
```rs
    fn main() {
        let mut v = Vec::new();

        v.push(5);
        v.push(6);
        v.push(7);
        v.push(8);
    }
```
### Reading Elements of Vectors;

There are two ways to reference a value stored in a vector.
```rs
    fn main() {
        let v = vec![1, 2, 3, 4, 5];

        let third: &i32 = &v[2];
        println!("The third element is {}", third);

        match v.get(2) {
            Some(third) => println!("The third element is {}", third),
            None => println!("There is no third element."),
        }
    }
```
But take care of adding a new element onto the end of the vector and accessing a value from vector becaue it might require allocating new memory and copying the old elements to the new space, if there isn’t enough room to put all the elements next to each other where the vector currently is. In that case, the reference to the first element would be pointing to deallocated memory. The borrowing rules prevent programs from ending up in that situation.

### Iterating over the Values in a Vector;
```rs
    fn main() {
        let v = vec![100, 32, 57];
        for i in &v {
            println!("{}", i);
        }
    }
```
---
We can also iterate over mutable references to each element in a mutable vector in order to make changes to all the elements.
```rs
    fn main() {
        let mut v = vec![100, 32, 57];
        for i in &mut v {
            *i += 50; // derefrece to access actual insted of refrence;
        }
    }
```
### Using an Enum to Store Multiple Types:

Using enum we can make a vector to carry multiple data type.
```rs
    fn main() {
        enum SpreadsheetCell {
            Int(i32),
            Float(f64),
            Text(String),
        }

        let row = vec![
            SpreadsheetCell::Int(3),
            SpreadsheetCell::Text(String::from("blue")),
            SpreadsheetCell::Float(10.12),
        ];
    }

```
# Hash Maps;
Note:"HashMap data Store in heap memory so we have to use & in loop , functions or slicing."
 
Stores a mapping of keys of type K to values of type V. It does this via a hashing function, which determines how it places these keys and values into memory. same as dictionary in Python.
```rs
    fn main() {
        use std::collections::HashMap;

        let mut scores = HashMap::new();

        scores.insert(String::from("Blue"), 10);
        scores.insert(String::from("Yellow"), 50);
    }
```
hash maps are homogeneous: all of the keys must have the same type, and all of the values must have the same type
Another way of constructing a hash map is by using iterators and the collect method on a vector of tuples, where each tuple consists of a key and its value. 
```rs
    fn main() {
        use std::collections::HashMap;

        let teams = vec![String::from("Blue"), String::from("Yellow")];
        let initial_scores = vec![10, 50];

        let mut scores: HashMap<_, _> =      
            teams.into_iter().zip(initial_scores.into_iter()).collect(); // hashmap<_,_> => key and value of any kind.
    }
```
### Accessing Values in a Hash Map;

We can get a value out of the hash map by providing its key to the get method
```rs
    fn main() {
        use std::collections::HashMap;

        let mut scores = HashMap::new();

        scores.insert(String::from("Blue"), 10);
        scores.insert(String::from("Yellow"), 50);

        let team_name = String::from("Blue");
        let score = scores.get(&team_name);
        
        if let Some(10) = scores {
            10
        }
    }
```
### Overwriting a Value;
 
 If we insert a key and a value into a hash map and then insert that same key with a different value, the value associated with that key will be replaced.
```rs
    fn main() {
        use std::collections::HashMap;

        let mut scores = HashMap::new();

        scores.insert(String::from("Blue"), 10);
        scores.insert(String::from("Blue"), 25);

        println!("{:?}", scores);
    }
```
### Only Inserting a Value If the Key Has No Value;
Hash maps have a special API for this called entry that takes the key you want to check as a parameter. The return value of the entry method is an enum called Entry that represents a value that might or might not exist.
```rs
    fn main() {
        use std::collections::HashMap;

        let mut scores = HashMap::new();
        scores.insert(String::from("Blue"), 10);

        scores.entry(String::from("Yellow")).or_insert(50);
        scores.entry(String::from("Blue")).or_insert(50);

        println!("{:?}", scores);
    }
```
### Updating a Value Based on the Old Value
Another common use case for hash maps is to look up a key’s value and then update it based on the old value. 

```rs
    fn main() {
        use std::collections::HashMap;
    (     let text = "hello world wonderful world";

        let mut map = HashMap::new();

        for word in text.split_whitespace() {
            let count = map.entry(word).or_insert(0);
            *count += 1; // Yeh  cheez smhjni hai
        }

        println!("{:?}", map);
    }
```

for more details check [Hello Peter 😎](https://doc.rust-lang.org/stable/book/ch08-00-common-collections.html)