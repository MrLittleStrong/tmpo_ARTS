###问题描述

在GitLab上添加了SSH的Key，但是在本机上使用git的SSH方法连，会报错

```
no matching host key type found. Their offer: ssh-dss
```

### 原因

之所以报错是因为OpenSSH 7.0以后的版本不再支持ssh-dss (DSA)算法，官方的说法是这个[算法太弱](https://link.jianshu.com/?t=https://www.openssh.com/legacy.html)了。

MAC OS升到10.12附带的openssh版本是7.4, 如下：

```shell
➜  ~ sshd -V
sshd: illegal option -- V
OpenSSH_7.4p1, LibreSSL 2.5.0
usage: sshd [-46DdeiqTt] [-C connection_spec] [-c host_cert_file]
            [-E log_file] [-f config_file] [-g login_grace_time]
            [-h host_key_file] [-o option] [-p port] [-u len]
```

### 解决办法

1. 命令行里添加选项
   `ssh -oHostKeyAlgorithms=+ssh-dss user@legacyhost`

2. 添加`HostKeyAlgorithms +ssh-dss`到配置`~/.ssh/config`

   ```shell
   Host *
       HostKeyAlgorithms +ssh-dss
   ```

   当然，上方的*可以改成你的gitlab的地址

### 参考

- [SSH登录失败：no matching host key type found. Their offer: ssh-dss](https://www.jianshu.com/p/6e5a323dcd9c)
- [SSH DSA keys no longer work for password-less authentication](https://superuser.com/questions/1016989/ssh-dsa-keys-no-longer-work-for-password-less-authentication)

