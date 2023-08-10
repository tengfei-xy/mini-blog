# yum安装

## 目录

-   [Instructions](#Instructions)

## Instructions

You can get started in three easy steps:

```纯文本
# 1. Install a package with repository for your system:
# On CentOS, install package centos-release-scl available in CentOS repository:
$ sudo yum install centos-release-scl

# On RHEL, enable RHSCL repository for you system:
$ sudo yum-config-manager --enable rhel-server-rhscl-7-rpms

# 2. Install the collection:
$ sudo yum install rh-postgresql96

# 3. Start using software collections:
$ scl enable rh-postgresql96 bash
```

At this point you should be able to use PostgreSQL just as a normal application. Here are some examples of commands you can run:

```纯文本
$ postgresql-setup --initdb
$ service rh-postgresql96-postgresql start
$ psql
```

Since su and sudo commands clear environment variables, we need to run scl enable once again for example after switching to postgres user role:

```纯文本
$ su - postgres -c 'scl enable rh-postgresql96 -- psql'
```

In order to view the individual components included in this collection, including additional subpackages, you can run:

```纯文本
$ sudo yum list rh-postgresql96\*
```
