
- using match to move the parse result to the variable `guess`:

```
let guess: u32 = match guess.trim().parse() {

Ok(num) => num,

Err(_) => continue,

};
```

- This code uses continue to ignore a loop iteration and return the number if the parse is successful.