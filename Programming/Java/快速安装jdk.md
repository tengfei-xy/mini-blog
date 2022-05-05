登录oracle方式获取

https://www.oracle.com/ca-en/java/technologies/javase/javase8u211-later-archive-downloads.html

其他链接

```
https://repo.huaweicloud.com/java/jdk/
```



直接下载

```
https://javadl.oracle.com/webapps/download/GetFile/1.8.0_321-b07/df5ad55fdd604472a86a45a217032c7d/linux-i586/jdk-8u321-linux-x64.tar.gz
```



```
sudo tar zxf jdk-8u321-linux-x64.tar.gz -C /usr/local

sudo sh -c "echo '
PATH=/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin
export JAVA_HOME=/usr/local/jdk1.8.0_321
export PATH=\$PATH:\$JAVA_HOME/bin
'>> /etc/profile"
. /etc/profile
java -version
```



