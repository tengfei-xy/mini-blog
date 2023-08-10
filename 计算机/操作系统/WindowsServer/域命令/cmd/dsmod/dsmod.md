# dsmod

要将**用户Mike Danseglio**添加到所有管理员分发列表**组**，请键入：

```纯文本
dsquery group "OU=Distribution Lists,DC=contoso,DC=com" -name  adm* | dsmod group -addmbr  "CN=Mike Danseglio,CN=Users,DC=contoso,DC=com"
```

要将“美国信息”组的**所有成员**添加到 添加到 **“加拿大信息”组**\*\*“加拿大信息”组\*\*，请键入：，请键入：

```纯文本
dsget group  "CN=US INFO,OU=Distribution Lists,DC=contoso,DC=com"  -members | dsmod group  "CN=CANADA INFO,OU=Distribution Lists,DC= contoso,DC=com"  -addmbr
```

要将**多个组**的组类型从安全性转换为非安全性，请键入：

```纯文本
dsmod group  "CN=US Info,OU=Distribution Lists,DC=Contoso,DC=Com"  "CN=Canada Info,OU=Distribution Lists,DC=Contoso,DC=Com"  "CN=Mexico Info,OU=Distribution Lists,DC=Contoso,DC=Com"  -secgrp no
```

要向组“CN=US Info，OU=Distribution Lists，DC=Contoso，DC=Com”**添加两个新成员**，请键入：

```纯文本
dsmod group  "CN=US Info,OU=Distribution Lists,DC=Contoso,DC=Com"  -addmbr "CN=Mike Danseglio,CN=Users,DC=Contoso,DC=Com"  "CN=Legal,OU=Distribution Lists,DC=Contoso,DC=Com"  "CN=Denise Smith,CN=Users,DC=Contoso,DC=Com"
```

要将Marketing组织单位（OU）中的**所有用户**添加到现有**组**Marketing Staff，请键入：

```纯文本
dsquery user OU=Marketing,DC=Contoso,DC=Com | dsmod group "CN=Marketing Staff,OU=Marketing,DC=Contoso,DC=Com" -addmbr
```

要从现有组Marketing Staff中删除Marketing组织单位（OU）中的**用户**，请键入：

```纯文本
dsquery user OU=Marketing,DC=Contoso,DC=Com | dsmod group "CN=Marketing Staff,OU=Marketing,DC=Contoso,DC=Com" -rmmbr
```

要从现有组“CN = US Info，OU = Distribution Lists，DC = Contoso，DC = Com”中删除**两个成员**，请键入：

```纯文本
dsmod group  "CN=US Info,OU=Distribution Lists,DC=Contoso,DC=Com"  -rmmbr "CN=Mike Danseglio,CN=Users,DC=Contoso,DC=Com"  "CN=Legal,OU=Distribution Lists,DC=Contoso,DC=Com"
```
