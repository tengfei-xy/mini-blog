# GoReleaser

快速开始：[参考](https://goreleaser.com/quick-start/ "参考")

-   发布
    1.  检查配置文件
        ```bash
        goreleaser check
        ```
    2.  设定环境变量GITHUB\_EXPORT并导出，token在[这里](https://github.com/login?return_to=https://github.com/settings/tokens/new "这里")创建，最小需要`write:packages`权限
    3.  在项目更新完毕之后，设置tag
        ```bash
        git tag -a v0.1.0 -m "First release"
        git push origin v0.1.0

        ```
    4.  创建发布
        ```bash
        goreleaser release
        ```
-   测试

    1.不发布release，生成测试二进制文件
    ```bash
    goreleaser build --snapshot --clean
    ```
