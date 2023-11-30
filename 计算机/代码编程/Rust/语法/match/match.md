# match

通用形式

```rust
match target {
    模式1 => 表达式1,
    模式2 => {
        语句1;
        语句2;
        表达式2
    },
    _ => 表达式3
}

```

取值

```rust
fn main() {
    let point = (3, 5);

    match point {
        (x, y) => {
            println!("x 坐标为: {}", x);
            println!("y 坐标为: {}", y);
        }
    }
}
// 运行结果为
// x 坐标为: 3
// y 坐标为: 5
```

单分支多模式的匹配

```rust
let x = 1;

match x {
    1 | 2 => println!("one or two"),
    3 => println!("three"),
    _ => println!("anything"),
}
```

序列匹配

```rust
let x = 5;

match x {
    1..=5 => println!("one through five"),
    _ => println!("something else"),
}
```

match的变量捕获用法，用于在模式匹配中捕获部分值，并在后续代码中重用它们。

```rust
fn main() {
    let number = Some(7);

    match number {
        Some(n @ 1..=5) => println!("Got a number between 1 and 5: {}", n),
        Some(n @ 6..=10) => println!("Got a number between 6 and 10: {}", n),
        Some(_) => println!("Got some other number"),
        None => println!("Got nothing"),
    }
}
```
