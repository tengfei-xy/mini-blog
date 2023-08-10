# msiexec

## 目录

-   [格式](#格式)
-   [安装选项](#安装选项)
-   [显示选项](#显示选项)
-   [重新启动选项](#重新启动选项)
-   [日志选项](#日志选项)
-   [更新选项](#更新选项)
-   [修复选项](#修复选项)

```纯文本
msiexec /i \\adserver\msi\puppet-agent-6.8.0-x64.msi /quiet /norestart /l\* desktop/log2.log PUPPET\_MASTER\_SERVER=puppet.monster.com
```

## 格式

msiexec /Option \<Required Parameter> \[Optional Parameter]

## 安装选项

\</package | /i> \<Product.msi>
安装或配置产品
/a \<Product.msi>
管理安装 - 在网络上安装产品
/j\<u|m> \<Product.msi> \[/t \<Transform List>] \[/g \<Language ID>]
公布产品 - m 公布到所有用户，u 公布到当前用户
\</uninstall | /x> \<Product.msi | ProductCode>
卸载产品

## 显示选项

/quiet
安静模式，无用户交互
/passive
无人参与模式 - 只显示进度栏
/q\[n|b|r|f]
设置用户界面级别
n - 无用户界面
b - 基本界面
r - 精简界面
f - 完整界面(默认值)
/help
帮助信息

## 重新启动选项

/norestart
安装完成后不重新启动
/promptrestart
必要时提示用户重新启动
/forcerestart
安装后始终重新启动计算机

## 日志选项

/l\[i|w|e|a|r|u|c|m|o|p|v|x|+|!|*] *\<LogFile>*
i - 状态消息
w - 非致命警告
e - 所有错误消息
a - 操作的启动
r - 操作特定记录
u - 用户请求
c - 初始用户界面参数
m - 内存不足或致命退出信息
o - 磁盘空间不足消息
p - 终端属性
v - 详细输出
x - 额外调试信息
\+ - 扩展到现有日志文件
! - 每一行刷新到日志
\* - 记录所有信息，除了 v 和 x 选项
/log *\<LogFile>*
与 /l* \<LogFile> 相同

## 更新选项

/update \<Update1.msp>\[;Update2.msp]
应用更新
/uninstall \<PatchCodeGuid>\[;Update2.msp] /package \<Product.msi | ProductCode>
删除产品的更新

## 修复选项

/f\[p|e|c|m|s|o|d|a|u|v] \<Product.msi | ProductCode>
修复产品
p - 仅当文件丢失时
o - 如果文件丢失或安装了更旧的版本(默认值)
e - 如果文件丢失或安装了相同或更旧的版本
d - 如果文件丢失或安装了不同版本
c - 如果文件丢失或较验和与计算的值不匹配
a - 强制重新安装所有文件
u - 所有必要的用户特定注册表项(默认值)
m - 所有必要的计算机特定注册表项(默认值)
s - 所有现有的快捷键方式(默认值)
v - 从源运行并重新缓存本地安装包
设置公共属性
\[PROPERTY=PropertyValue]
