---
date: 2012-02-01T17:34:03+08:00
title: 脚本中的sudo命令使用管道时的权限问题
---

在 shell 环境中，如果直接使用 sudo 命令将结果通过管道写入文件，会发生权限问题。例如

```
sudo echo "JAVA_HOME=/usr/local/jre" >> /etc/profile
```

其中，/etc/profile 的权限是 644，发生权限不足的报错。

这可能是由于 sudo 只针对执行命令权限，并没有针对写入文件权限。我们需要用 bash -c 方式包装整个命令执行(注意双引号的转义)

```
sudo bash -c "echo \"JAVA_HOME=/usr/local/jre\" >> /etc/profile"
```

另一种方法是采用 tee 命令

```
sudo echo "JAVA_HOME=/usr/local/jre" | sudo tee -a /etc/profile >  /dev/null
```

参考：[http://stackoverflow.com/questions/82256/how-do-i-use-sudo-to-redirect-output-to-a-location-i-dont-have-permission-to-wr]
