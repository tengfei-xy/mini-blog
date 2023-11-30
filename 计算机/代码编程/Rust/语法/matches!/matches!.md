# matches!

作用：将一个表达式跟模式进行匹配，然后返回匹配的结果 `true` or `false`。

示例

```rust
fn main() {
    let number = 42;

    if matches!(number, 1 | 2 | 3) {
        println!("The number is 1, 2, or 3");
    } else {
        println!("The number is not 1, 2, or 3");
    }
}
```
