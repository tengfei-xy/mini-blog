克隆

```bash
git clone https://github.com/oracle/docker-images.git
```

[构建帮助](https://github.com/oracle/docker-images/tree/main/OracleDatabase/SingleInstance)

将安装包放置于OracleDatabase/SingleInstance/dockerfiles/x.x.x.

| 版本     | 安装包名                     |
| -------- | ---------------------------- |
| 12.2.0.1 | linuxx64_12201_database.zip  |
| 19.3.0   | LINUX.X64_193000_db_home.zip |

构建镜像

```bash
dockerfiles/buildContainerImage.sh  -v x.x.x.x -e
```

- 一般

  ```bash
  
  ```

- 多租户实例

  启动模板

  ```bash
  docker run --name <container name> \
  --shm-size=1g \
  -p 1521:1521 \
  -e ORACLE_PWD=<your database passwords> \
  -v [<host mount point>:]/u01/app/oracle/oradata \
  oracle/database:12.2.0.1-ee
  ```

  自定义启动

  ```bash
  docker run --name 12c-0001 \
  --shm-size=1g \
  -p 1001:1521 \
  -e ORACLE_PWD=bb123456 \
  oracle/database:12.2.0.1-ee
  
  sqlplus sys/bb123456@192.168.3.67/ORCLCDB as sysdba
  ```


- 其他选项

  ```bash
  docker run --name <container name> \
  -p <host port>:1521 -p <host port>:5500 \
  -e ORACLE_PWD=<your database passwords> \
  -e ORACLE_CHARACTERSET=<your character set> \
  -v [<host mount point>:]/opt/oracle/oradata \
  oracle/database:18.4.0-xe
  
  Parameters:
     --name:        The name of the container (default: auto generated)
     -p:            The port mapping of the host port to the container port.
                    Two ports are exposed: 1521 (Oracle Listener), 5500 (EM Express)
     -e ORACLE_PWD: The Oracle Database SYS, SYSTEM and PDB_ADMIN password (default: auto generated)
     -e ORACLE_CHARACTERSET:
                    The character set to use when creating the database (default: AL32UTF8)
     -v /opt/oracle/oradata
                    The data volume to use for the database.
                    Has to be writable by the Unix "oracle" (uid: 54321) user inside the container!
                    If omitted the database will not be persisted over container recreation.
     -v /opt/oracle/scripts/startup | /docker-entrypoint-initdb.d/startup
                    Optional: A volume with custom scripts to be run after database startup.
                    For further details see the "Running scripts after setup and on startup" section below.
     -v /opt/oracle/scripts/setup | /docker-entrypoint-initdb.d/setup
                    Optional: A volume with custom scripts to be run after database setup.
                    For further details see the "Running scripts after setup and on startup" section below.
  ```

  
