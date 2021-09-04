# Enums;
Useful and more appropriate than structs. Using _enum_ we can specify each member of a sequence individually.
we declare only one member of enum at a time.
declaring as  
let variable_name = enum_name::member;

    enum IpAddrKind {
        V4,
        V6,
    }

    fn main() {
        let four = IpAddrKind::V4;
        let six = IpAddrKind::V6;
    }

### Enum in Struct;

    fn main() {
        enum IpAddrKind {
            V4,
            V6,
        }

        struct IpAddr {
            kind: IpAddrKind,
            address: String,
        }

        let home = IpAddr {
            kind: IpAddrKind::V4,
            address: String::from("127.0.0.1"),
        };

        let loopback = IpAddr {
            kind: IpAddrKind::V6,
            address: String::from("::1"),
        };
    }
*__or Simply without Struct's help__*.

    fn main() {
    enum IpAddr {
        V4(String),
        V6(String),
    }

    let home = IpAddr::V4(String::from("127.0.0.1"));

    let loopback = IpAddr::V6(String::from("::1"));
}

### Struct in Enum ;

    #![allow(unused)]
    fn main() {
    struct Ipv4Addr {
        // --snip--
    }

    struct Ipv6Addr {
        // --snip--
    }

    enum IpAddr {
        V4(Ipv4Addr),
        V6(Ipv6Addr),
    }
    }

### Defining Methods;

Same as Struct under impl enum block.

    fn main() {
        enum Message {
            Quit,
            Move { x: i32, y: i32 },
            Write(String),
            ChangeColor(i32, i32, i32),
        }

    impl Message {
        fn call(&self) {
            // method body would be defined here
        }
    }

    let m = Message::Write(String::from("hello"));
    m.call();
}

## The Option Enum ;
Basic knowledge : use in place of null to save a variable having nothing in it.


    #![allow(unused)]
    fn main() {
    enum Option<T> {
        Some(T),
        None,
    }
    }
------
    fn main() {
        let some_number = Some(5);
        let some_string = Some("a string");

    let absent_number: Option<i32> = None;
    }
### The Match Control Flow Operator;

Match allows us to compare a value against a series of patterns and then execute code based on which pattern matches. Patterns can be made up of literal values. The power of match comes from the expressiveness of the patterns and the fact that the compiler confirms that all possible cases are handled. Similar to if_statement but not completely, if statement expression needs to return a Boolean value, but here, it can be any type.

    enum Coin {
        Penny,
        Nickel,
        Dime,
        Quarter,
    }

    fn value_in_cents(coin: Coin) -> u8 {
        match coin {
            Coin::Penny => 1(// it can be any thing  ),
            Coin::Nickel => 5,
            Coin::Dime => 10,
            Coin::Quarter => 25,
        }
    }

    fn main() { 
        let sikka = Coin::Peeny;
        value_in_cents(sikka); 

    }
---
Enum1 member can be a data type of other Enum2 member
    #[derive(Debug)] // so we can inspect the state in a minute
    enum UsState {
        Alabama,
        Alaska,
        // --snip--
    }

    enum Coin {
        Penny,
        Nickel,
        Dime,
        Quarter(UsState),
    }

    fn main() {

        let _c = Coin::Quarter(UsState::Alabama);
    }

---

    #[derive(Debug)]
    enum UsState {
        Alabama,
        Alaska,
        // --snip--
    }

    enum Coin {
        Penny,
        Nickel,
        Dime,
        Quarter(UsState),
    }

    fn value_in_cents(coin: Coin) -> u8 {
        match coin {
            Coin::Penny => 1,
            Coin::Nickel => 5,
            Coin::Dime => 10,
            Coin::Quarter(state) => {
                println!("State quarter from {:?}!", state);
                25
            }
        }
    }

    fn main() {
        value_in_cents(Coin::Quarter(UsState::Alaska));
    }
### Concise Control Flow with if let;

The if let syntax lets you combine if and let into a less verbose way to handle values that match one pattern while ignoring the rest.

working as => if let "Equal to condition" { code block }


    fn main() {
        let some_u8_value = Some(0u8);
        if let Some(3) = some_u8_value {
            println!("three");
        }
    }

the code ,

      

    fn main() {
        let coin = Coin::Penny;
        let mut count = 0;
        match coin {
            Coin::Quarter(state) => println!("State quarter from {:?}!", state),
            _ => count += 1,
        }
    }

can be written with the help of 'else' as 


    fn main() {
        let coin = Coin::Penny;
        let mut count = 0;
        if let Coin::Quarter(state) = coin {
            println!("State quarter from {:?}!", state);
        } else {
            count += 1;
        }
    }
For more details check [Hello Peter 😎](https://doc.rust-lang.org/stable/book/ch06-00-enums.html)
