# mydumper

## 目录

-   [myloader](#myloader)

<https://github.com/mydumper/mydumper>

下载，注：安装完成后将包含mydumer和myloader

```shell
release=$(curl -Ls -o /dev/null -w %{url_effective} https://github.com/mydumper/mydumper/releases/latest | cut -d'/' -f8)
sudo yum install https://github.com/mydumper/mydumper/releases/download/${release}/mydumper-${release:1}.el7.x86_64.rpm
```

用法：[https://github.com/mydumper/mydumper/blob/master/docs/mydumper\_usage.rst](https://github.com/mydumper/mydumper/blob/master/docs/mydumper_usage.rst "https://github.com/mydumper/mydumper/blob/master/docs/mydumper_usage.rst")

```shell
# 导出到-o选项的文件夹
sudo mydumper --host=192.168.0.125 --user=root --password=password --threads=16 --database data_manage -T data_manage.fans_server_user_request_logger_202305 -c --verbose=3 -o data_manage.fans_server_user_request_logger_202305
```

> 16线程，接近1亿8kw条数据，11分钟
> Intel(R) Xeon(R) CPU E5-2650 v3 @ 2.30GHz

## myloader

用法：[https://github.com/mydumper/mydumper/blob/master/docs/myloader\_usage.rst](https://github.com/mydumper/mydumper/blob/master/docs/myloader_usage.rst "https://github.com/mydumper/mydumper/blob/master/docs/myloader_usage.rst")

```shell
sudo myloader --host=127.0.0.1 --user=root --password password --threads=16 --overwrite-tables  --directory=export-20230410-095113
```

-   将备份的文件转换为.sql文件
    ```shell
    # --directory 指定备份的路径
    # 尾部的参数 指定输出.sql的文件夹
    myloader --host=192.168.0.37 --user=root --password=password --threads=16 --database data_manage --directory=export-20230407-153747 data_manage_fans_server_user_request_logger
    ```
