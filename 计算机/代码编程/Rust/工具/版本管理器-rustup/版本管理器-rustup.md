# 版本管理器-rustup

-   安装rustup
    > 📌需要翻墙
    -   Linux/macOS
        ```bash
        curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
        ```
    -   windows

        [https://course.rs/first-try/installation.html#在-windows-上安装-rustup](https://course.rs/first-try/installation.html#在-windows-上安装-rustup "https://course.rs/first-try/installation.html#在-windows-上安装-rustup")

更新

```bash
rust update
```

卸载（rust和rustup）

```bash
rust self uninstall
```

-   查看版本
    ```bash
    $ rustc -V
    rustc 1.56.1 (59eed8a2a 2021-11-01)

    $ cargo -V
    cargo 1.57.0 (b2e52d7ca 2021-10-21)
    ```
