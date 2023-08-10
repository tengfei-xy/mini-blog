# REG

## 目录

-   [查找](#查找)
-   [修改](#修改)

## 查找

REG QUERY KeyName \[/v \[ValueName] | /ve] \[/s]
\[/f Data \[/k] \[/d] \[/c] \[/e]] \[/t Type] \[/z] \[/se Separator]

KeyName  \[\\\Machine\\]FullKey
Machine - 远程机器名称，省略当前机器的默认值。在远程机器上
只有 HKLM 和 HKU 可用。
FullKey - 以 ROOTKEY\SubKey 名称形式
ROOTKEY - \[ HKLM | HKCU | HKCR | HKU | HKCC ]
SubKey  - 在选择的 ROOTKEY 下的注册表项的全名

/v       具体的注册表项值的查询。
如果省略，会查询该项的所有值。只有与 /f 开关一起指定的情况下，此开关的参数才是可选的。它指定
只在值名称中搜索。

/ve      查询默认值或空值名称(默认)。

/s       循环查询所有子项和值(如 dir /s)。

/se      为 REG\_MULTI\_SZ 在数据字符串中指定分隔符(长度只为 1 个字符)。
默认分隔符为 "\0"。

/f       指定搜索的数据或模式。
如果字符串包含空格，请使用双引号。默认为 " \*"。/f       指定搜索的数据或模式。
如果字符串包含空格，请使用双引号。默认为 " \*"。

/k       指定只在项名称中搜索。

/d       指定只在数据中搜索。

/c       指定搜索时区分大小写。
默认搜索为不区分大小写。

/e       指定只返回完全匹配。
默认是返回所有匹配。

/t       指定注册表值数据类型。
有效的值是:
REG\_SZ, REG\_MULTI\_SZ, REG\_EXPAND\_SZ,
REG\_DWORD, REG\_QWORD, REG\_BINARY, REG\_NONE
默认为所有类型。

/z       详细: 显示值名称类型的数字等值。

示例:

REG QUERY HKLM\Software\Microsoft\ResKit /v Version
显示注册表值版本的值

REG QUERY \ABC\HKLM\Software\Microsoft\ResKit\Nt\Setup /s
显示远程机器 ABC 上的、在注册表项设置下的所有子项和值

REG QUERY HKLM\Software\Microsoft\ResKit\Nt\Setup /se #
用 "#" 作为分隔符，显示类型为 REG\_MULTI\_SZ 的所有值名称的所有
子项和值。

REG QUERY HKLM /f SYSTEM /t REG\_SZ /c /e
以区分大小写的形式显示项、值和数据和数据类型 REG\_SZ
的、在 HKLM 更目录下的、"SYSTEM" 出现的精确次数

REG QUERY HKCU /f 0F /d /t REG\_BINARY
显示在 HKCU 根目录下、数据类型为 REG\_BINARY 的数据的项、值和
数据的 "0F" 出现的次数。

REG QUERY HKLM\SOFTWARE /ve
显示在 HKLM\SOFTWARE 下的项、值和数据(默认)

## 修改

```纯文本
reg add 
```
