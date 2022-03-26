# psexec 常用实例锦集

[PsExec官方文档](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec)

注：绝大多数情况都需要启动ADMIN$共享,查看方式:`net share`





执行远程主机的文件，在tx16-pc的Session 1上以管理员权限运行本地的FFCell.exe，并立刻返回。

```
psexec \\NB-JL-014-PC -s -d -i 1 -c H:\windows\share\FFCell.exe
```

将本地主机的文件复制到远程主机再运行

```
psexec \\NB-JL-014-PC -nobanner -s -f -c C:\Users\Administrator\Desktop\bat\computer\updateIP.bat
```



## 说明

 ```
 psexec [\\computer[,computer2[,...] | @file\]][-u user [-p psswd][-n s][-r servicename][-h][-l][-s|-e][-x][-i [session]][-c executable [-f|-v]][-w directory][-d][-<priority>][-a n,n,...] cmd [arguments]
 ```



| Parameter       | Description                                                  |
| :-------------- | :----------------------------------------------------------- |
| **-a**          | 应用程序可以使用逗号运行的单独处理器，其中1是编号最低的 CPU。 例如，要在 CPU 2和 CPU 4上运行应用程序，请输入: “-a 2,4” |
| **-c**          | 将指定的可执行文件复制到远程系统以执行。 如果省略此选项，则应用程序必须位于远程系统的系统路径中 |
| **-d**          | 不要等待进程终止(非交互式)                                   |
| **-e**          | 不加载指定帐户的配置文件                                     |
| **-f**          | 复制指定的程序，即使该文件已经存在于远程系统中               |
| **-i**          | 运行程序，使其与远程系统上指定会话的桌面进行交互。 如果没有指定会话，则进程在控制台会话中运行 |
| **-h**          | 如果目标系统是 Vista 或更高版本，则使用帐户的提升令牌运行进程(如果可用的话) |
| **-l**          | 以受限用户身份运行进程(取消 Administrators 组并只允许分配给 Users 组的特权)。 在 windowsvista 上，该进程以低完整性运行 |
| **-n**          | 指定连接到远程计算机的超时(以秒为单位)                       |
| **-p**          | 指定用户名的可选密码。如果省略此项，将提示您输入隐藏密码     |
| **-r**          | 指定要创建或与之交互的远程服务的名称                         |
| **-s**          | 在系统帐户中运行远程进程                                     |
| **-u**          | 指定登录到远程计算机的可选用户名                             |
| **-v**          | 只有当指定的文件的版本号较高或者比远程系统上的文件的版本号较新时，才复制该文件 |
| **-w**          | 设置进程的工作目录值(相对于远程计算机)                       |
| **-x**          | 在 Winlogon 安全桌面上显示 UI (仅限于本地系统)               |
| **-priority**   | 指定-低,-低于正常,-高于正常,-高或-实时以不同的优先级运行进程。 使用后台运行在低内存和 i / o 优先级的 Vista |
| **computer**    | 指定 PsExec 在指定的远程计算机上运行应用程序。 如果省略计算机名，则 PsExec 将在本地系统上运行应用程序，如果指定通配符( *) ，则 PsExec 将在当前域中的所有计算机上运行该命令 |
| **@file**       | 将在文件中列出的每台计算机上执行该命令                       |
| **cmd**         | 要执行的应用程序名称                                         |
| **arguments**   | 要传递的参数(请注意，文件路径必须是目标系统上的绝对路径)     |
| **-accepteula** | 此标志将抑制许可对话框的显示                                 |

You can enclose applications that have spaces in their name with quotation marks e.g.

**psexec \\marklap"c:\long name app.exe"**

Input is only passed to the remote system when you press the Enter key. Typing Ctrl-C terminates the remote process.

If you omit a user name, the process will run in the context of your account on the remote system, but will not have access to network resources (because it is impersonating). Specify a valid user name in the Domain\User syntax if the remote process requires access to network resources or to run in a different account. Note that the password and command are encrypted in transit to the remote system.

Error codes returned by PsExec are specific to the applications you execute, not PsExec.