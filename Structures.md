# Structures:
Structs are similar to tuples , can have diff pieces of data but you can name the pieces. so don't need to rely on order of data.

    struct User {
        username: String,
        email: String,
        sign_in_count: u64,
        active: bool,
    }

    fn main() {
      
     let user1 = User {
        email: String::from("someone@example.com"),
        username: String::from("someusername123"),
        active: true,
        sign_in_count: 1,
        };
    }



we can access any data from Struct by dot notation or even can update if we declare struct variable as mutable.

    struct User {
        username: String,
        email: String,
        sign_in_count: u64,
        active: bool,
    }

    fn main() {
        let mut user1 = User {
            email: String::from("someone@example.com"),
            username: String::from("someusername123"),
            active: true,
            sign_in_count: 1,
        };

        user1.email = String::from("anotheremail@example.com");
    }

### Creating Instances From Other Instances With Struct Update Syntax;    
    
    
    
      
    struct User {
        username: String,
        email: String,
        sign_in_count: u64,
        active: bool,
    }

    fn main() {
        let user1 = User {
            email: String::from("someone@example.com"),
            username: String::from("someusername123"),
            active: true,
            sign_in_count: 1,
        };

        let user2 = User {
            email: String::from("another@example.com"),
            username: String::from("anotherusername567"),
            active: user1.active,
            sign_in_count: user1.sign_in_count,
        };
    }

__OR__ ( using ' .. ' syntax)


    struct User {
        username: String,
        email: String,
        sign_in_count: u64,
        active: bool,
    }

    fn main() {
        let user1 = User {
            email: String::from("someone@example.com"),
            username: String::from("someusername123"),
            active: true,
            sign_in_count: 1,
        };

        let user2 = User {
            email: String::from("another@example.com"),
            username: String::from("anotherusername567"),
            ..user1
        };
    }

### Ownership of Struct Data;
 we used the owned String type rather than the &str string slice type, because we want instances of this struct to own all of its data and for that data to be valid for as long as the entire struct is valid.

     username: &str,
    |               ^ expected named lifetime parameter

    * lifetime parameter k baare mai prhna hai.
  
  ### Sample Code :
    struct Rectangle {
        width: u32,
        height: u32,
    }

    fn main() {
        let rect1 = Rectangle {
            width: 30,
            height: 50,
        };

        println!(
            "The area of the rectangle is {} square pixels.",
            area(&rect1)
        );
    }

    fn area(rectangle: &Rectangle) -> u32 {
       // "&Rectangle" because we don't want struct ownership
        rectangle.width * rectangle.height
    }

### Defining Methods;
We can directly define a function on the Struct. using impl Struct block .
Function can be called by instance.fuction() or Struct::function(instance).


    #[derive(Debug)]
    struct Rectangle {
        width: u32,
        height: u32,
    }

    impl Rectangle {
        fn area(&self) -> u32 {
            self.width * self.height
        }
    }

    fn main() {
        let rect1 = Rectangle {
            width: 30,
            height: 50,
        };

        println!(
            "The area of the rectangle is {} square pixels.",
            rect1.area() 
            or 
            Rectangle::area(&rect1)
        );
    }

### Methods with More Parameters;

    #[derive(Debug)]
    struct Rectangle {
        width: u32,
        height: u32,
    }

    impl Rectangle {
        fn area(&self) -> u32 {
            self.width * self.height
        }

        fn can_hold(&self, other: &Rectangle) -> bool {
            self.width > other.width && self.height > other.height
        }
    }

    fn main() {
        let rect1 = Rectangle {
            width: 30,
            height: 50,
        };
        let rect2 = Rectangle {
            width: 10,
            height: 40,
        };
        let rect3 = Rectangle {
            width: 60,
            height: 45,
        };

        println!("Can rect1 hold rect2? {}", rect1.can_hold(&rect2));
        println!("Can rect1 hold rect3? {}", rect1.can_hold(&rect3));
    }


For more Details check [Hello Peter 😎](https://doc.rust-lang.org/book/ch05-00-structs.html, "ڈھانچہ")
