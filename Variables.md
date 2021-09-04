# Variables

## Mutability:


**_Variable_** should declared as mutable (mut) in case of you wanted to change it or upgrade it later. 

    
    let mut x = 5;
    println!("The value of x is: {}", x);
    x = 6;
    println!("The value of x is: {}", x);

You can also declare a variable constant (const without let) to keep that variable unchangeable throughout program.

    
    
    fn main() {
    const MAX_POINTS: u32 = 100_000;
    }  
it is neccessary to annocate the variable

## Shadowing:
 
    fn main() {
    let x = 5;

    let x = x + 1;

    let x = x * 2;

    println!("The value of x is: {}", x);
    }

In this case we are not upgrading the variable we are just initilizing new variable with the same name .

We can also change a variable data type by shadowing if we don't annocated it data type at that time of initializing.

    let mut number = "T-H-R-E-E"; // don't change this line
    println!("Spell a Number : {}", number);
    let number = 3;
    println!("Number plus two is : {}", number + 2);


For more details check [Hello Peter 😎](https://doc.rust-lang.org/book/ch03-01-variables-and-mutability.html,  "Variables And their Harkats")

