---
title: SSH 免密登录
date: 2021-12-03
tags: ["linux"]
---
## 在客户端

### Linux

```shell
$ ssh-keygen
Generating public/private rsa key pair.
...
$ ls
id_rsa  id_rsa.pub  known_hosts
```

#### 简单方式 `ssh-copy-id` 命令， 如果此方式成功就不用看下面了～

`ssh-copy-id root@192.168.88.78`　
![20211204_GYfqdt](https://cdn.jsdelivr.net/gh/crexk/pic-archive@main/uPic/20211204_GYfqdt.png)

### Windows


```shell
type $env:USERPROFILE.ssh\id_rsa.pub | ssh root@192.168.30.31 "cat >> .ssh/authorized_keys"
```

## 在服务器端：

将第一步生成的 `id_rsa.pub` 内容写入到
`~/.ssh/authorized_keys`中

**注意**

如果登录普通用户，如 `gisviewer`: 
则在 `/home/gisviewer/.ssh/authorized_keys` 中

如果是`root`,
在 `/root/.ssh/authorized_keys` 中

![](https://cdn.jsdelivr.net/gh/crexk/pic-archive@main/picgo/20211203163955.png)

## 修改服务端ssh配置

`vim /etc/ssh/sshd_config`

```
PasswordAuthentication yes　　　　　　# 口令登录
RSAAuthentication yes　　　　　　　　　# RSA认证
PubkeyAuthentication yes　　　　　　　# 公钥登录 
PermitRootLogin yes                    # Root 登录
```
## 重启ssh服务

`service ssh restart`

## 客户端用私钥登录

`ssh -i id_rsa root@192.168.88.78`

![](https://cdn.jsdelivr.net/gh/crexk/pic-archive@main/picgo/20211203164148.png)

## 配置config使用简称登录

上面操作完成后，登录服务器还是需要输入完整ip，可以通过config来简化命令

Windows的配置文件目录在 `C:\Users\{user}\.ssh\config`

```bash
Host 101
    HostName 192.168.56.101
    User root
    Port 22
```

配置好后 

`ssh 101` 即可登录 `192.168.56.101` 的机器。

## ssh 端口代理

```
ssh -D 11080 root@192.168.56.101
```

使用命令 `ssh -D 11080 root@192.168.56.101` 可以建立到远程服务器 `192.168.56.101` 的 SSH 连接，同时在本地机器上配置一个监听在端口 `11080` 的 SOCKS 代理。

以下是该命令的详细说明：

- `ssh`：用于启动连接的命令。
- `-D 11080`：该选项指定动态应用层端口转发，设置一个本地监听在端口 `11080` 的 SOCKS 代理服务器。
- `root@192.168.56.101`：远程服务器的用户名（`root`）和 IP 地址（`192.168.56.101`）。

这种设置可以用于通过远程服务器安全地隧道流量。例如，你可以将你的网页浏览器配置为使用 `localhost:11080` 的 SOCKS 代理，从而通过远程服务器路由你的网页流量。

### 重要事项：
1. **安全性**：通常不推荐使用 `root` 进行 SSH 连接，因为这存在比较高的安全风险。建议使用权限较低的普通用户帐户，如果需要管理员权限，可以考虑使用 `sudo`。
2. **防火墙和网络配置**：确保远程服务器上的端口 22（默认的 SSH 端口）是打开的，并且必要的防火墙规则配置正确。
3. **SSH 密钥认证**：如果可能，建议使用 SSH 密钥认证代替密码认证，以提高安全性。
4. **代理配置**：建立连接后，配置你的应用程序（如网页浏览器）使用 `localhost:11080` 的 SOCKS 代理。

### 网页浏览器（Firefox）代理配置示例：
1. 打开 Firefox，进入 `选项` 或 `设置`。
2. 滚动到 `网络设置` 部分并点击 `设置`。
3. 选择 `手动代理配置`。
4. 在 `SOCKS 主机` 字段中输入 `localhost`，在 `端口` 字段中输入 `11080`。
5. 勾选 `SOCKS v5`。
6. 点击 `确定` 保存设置。

这样设置之后，你的网页流量就会通过 SSH 隧道路由，提供更安全的浏览体验。