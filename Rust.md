
- using match to move the parse result to the variable `guess`:

```
let guess: u32 = match guess.trim().parse() {

Ok(num) => num,

Err(_) => continue,

};
```

- This code uses continue to ignore a loop iteration and return the number if the parse is successful.


# Data types

- Writing an array with type
	- `let a: [i32; 5] = [1, 2, 3, 4, 5];
	- Type of int32 with 5 elements


# Functions 
- To return a value from a function just end the line without a semicolon ";"