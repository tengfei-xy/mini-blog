# DRA处理介质失效指令

## 目录

-   [查看故障](#查看故障)
-   [提供建议](#提供建议)
-   [修复故障](#修复故障)
-   [修改故障优先级](#修改故障优先级)

## 查看故障

检查问题

```纯文本
RMAN> list failure;
```

验证情况

```bash
validate database
```

## 提供建议

查看Failure ID的更多信息

```纯文本
RMAN> list failure <ID> defail;
```

获取建议

```纯文本
RMAN> advise failure 6222;
```

## 修复故障

预览 REPAIR FAILURE操作

```bash
RMAN> repair failure previrew;
```

如果接受

```bash
RMAN> repair failure;
```

## 修改故障优先级

```bash
RMAN> change failure <ID> priority low;
```
