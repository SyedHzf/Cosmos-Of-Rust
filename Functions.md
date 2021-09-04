# Functions
### Structure:
 Declared with __fn__ keyword + function name + (parameter : Parameter type) + -> + return data type

    fn sale_price(price: i32) -> i32 {
    if is_even(price) {
            price - 10
    } else {
            price - 3
        }
    }

    fn is_even(num: i32) -> bool {
        num % 2 == 0
    }

From above code if we replace **_price - 10_**  with  **_price - 10 ;_** it means we are changing expression into a statement . so that compiler wouldn't see it as a return data and error will occur.

    fn plus_one(x: i32) -> i32 {
    x + 1;

Error:

    fn plus_one(x: i32) -> i32 {
      |    --------            ^^^ expected `i32`, found `()`
    |    |
    |    implicitly returns `()` as its body has no tail or `return` expression


## Sample Function with If/else condition:

    fn calculate_apple_price(num: i32) -> i32{
        if num > 40 {
            num
        }else{
            num*2
        }
    }

    // Don't modify this function!
    #[test]
    fn verify_test() {
        let price1 = calculate_apple_price(35);
        let price2 = calculate_apple_price(40);
        let price3 = calculate_apple_price(65);

        assert_eq!(70, price1);
        assert_eq!(80, price2);
        assert_eq!(65, price3);
    }


For more Details check [Hello Peter 😎.](https://doc.rust-lang.org/book/ch03-03-how-functions-work.html, "Functions")