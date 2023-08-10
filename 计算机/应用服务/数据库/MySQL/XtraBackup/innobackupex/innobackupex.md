# innobackupex

## 目录

-   [innobackupex备份](#innobackupex备份)
    -   [全备份](#全备份)
    -   [增量备份](#增量备份)
    -   [增量恢复](#增量恢复)
    -   [另外](#另外)

# innobackupex备份

## 全备份

1.  开始备份
    `innobackupex --defaults-file=my.cnf --user=name --host=name --password= --no-timestamp --port=port --socket=socket  PATH`
2.  记录备份时变化的日志，回滚没有提交的事务日志数据
    `innobackupex --apply-log --use-memory=32M backup/`
3.  开始恢复
    关闭数据库
    innobackupex --defaults-file=./etc/my.cnf --copy-back --resync data/ -
    mv -
    修改所属者 -
    启动数据库 -

## 增量备份

基于全备份增加两个参数，要用到上一次备份路径
\--incremental
\--incremental-basedir=上一次备份路径

## 增量恢复

命令相同
innobackupex --apply-log --use-memory=32M backup/
如果不是最后一次合并增量数据，需要增加--redo-only参数
innobackupex --apply-log --use-memory=32M --incremental-dir=/backup/

## 另外

备份A和B表
\--databases="A B"

并行进程数
\--parallel=16

限制每秒IO次数
\--throttle=100
