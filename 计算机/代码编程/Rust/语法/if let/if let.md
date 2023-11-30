# if let

使用场景：仅有一个模式陪匹配时使用`if let`

```rust
if let Some(3) = v {
    println!("three");
}

// 等同于
let v = Some(3u8);
match v {
    Some(3) => println!("three"),
    _ => (),
}

```
