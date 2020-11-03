# bar中reg取值

实现的是一种使用reg命令，针对字符串类型的类型取值。

```bat
@echo off
set basepath=HKCU\software\ADControl
rem 从第二行开始，使用第三列的输出结果
for /F "skip=2 tokens=3" %%a in ('reg query %basepath% /v loginUser') do (
set LoginUser=%%a
)

echo %LoginUser%
pause
```

