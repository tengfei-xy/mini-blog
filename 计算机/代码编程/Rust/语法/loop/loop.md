# loop

> 📌loop 是一个表达式，因此可以返回一个值

一般格式

```rust
fn main() {
    let mut n = 0;

    loop {
        if n > 5 {
            break
        }
        println!("{}", n);
        n+=1;
    }

    println!("我出来了！");
}
```
