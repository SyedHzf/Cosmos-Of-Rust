# Modules:
Modules let us organize code within a crate into groups for readability and easy reuse. Modules also control the privacy of items, which is whether an item can be used by outside code (public) or is an internal implementation detail and not available for outside use (private).

    mod front_of_house {
        /-  // Absolute path
    crate::front_of_house::hosting::add_to_waitlist();

    // Relative path
    front_of_house::hosting::add_to_waitlist(); -/
        mod hosting {
            fn add_to_waitlist() {}

            fn seat_at_table() {}
        }

        mod serving {
            fn take_order() {}

            fn serve_order() {}

            fn take_payment() {}
        }
    }
---
#### _Module tree_:
    crate
    └── front_of_house
        ├── hosting
        │   ├── add_to_waitlist
        │   └── seat_at_table
        └── serving
            ├── take_order
            ├── serve_order
            └── take_payment

### Sample Code:
    mod  sausage_factory {
    
        pub fn  make_sausage() {
            println!("Hello Peter 😎!!");
        }
    }

    fn main() {
        sausage_factory::make_sausage();
    }
**And**

    pub mod delicious_snacks {
        pub mod fruits {
            pub const PEAR: &str = "Pear";
            pub const APPLE: &str = "Apple";
        }

        pub mod veggies {
            pub const CUCUMBER: &str = "Cucumber";
            pub const CARROT: &str = "Carrot";
        }
        pub use self::fruits::PEAR as fruit;
        pub use self::veggies::CUCUMBER as veggie;

    }

    fn main() {
        println!(
            "favorite snacks: {} and {}",
            delicious_snacks::fruits::PEAR ,
            delicious_snacks::veggie
        );
    }
for more details check [Hello Peter 😎](https://doc.rust-lang.org/stable/book/ch07-01-packages-and-crates.html)
