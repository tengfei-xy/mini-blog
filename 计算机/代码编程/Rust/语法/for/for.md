# for

序列循环

```rust
    for i in 1..=5 {
        println!("{}", i);
    }
    
    for _ in 0..10 {
        // ...
    }
```

不转移所有权的循环

```rust
for item in &container {
  // ...
}
```

在循环中修改元素

```rust
for item in &mut collection {
  // ...
}
```

循环中获取索引

```rust
    let a = [4, 3, 2, 1];
    // `.iter()` 方法把 `a` 数组变成一个迭代器
    for (i, v) in a.iter().enumerate() {
        println!("第{}个元素是{}", i + 1, v);
    }
```
