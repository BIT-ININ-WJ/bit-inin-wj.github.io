---
layout: post
title: linux服务器使用手册
date: 2024-08-16 17:32:13
description: linux服务器常用命令&常见问题
tags: linux command line
categories: sample-posts
tabs: true
---

## 用户登录

### SSH登录

{% tabs login %}

{% tab login windows终端 %}

<div class="col-sm mt-3 mt-md-0">
  {% include figure.liquid loading="eager" path="assets/img/posts/windows_terminal.png" class="img-fluid rounded z-depth-1" zoomable=true %}
</div>

{% endtab %}

{% tab login XShell %}

<div class="col-sm mt-3 mt-md-0">
  {% include figure.liquid loading="eager" path="assets/img/posts/xshell.png" class="img-fluid rounded z-depth-1" zoomable=true %}
</div>

{% endtab %}

{% tab login MobaXterm %}

[MobaXterm详细使用教程](https://blog.csdn.net/xuanying_china/article/details/120080644)
[MobaXterm快速入门、高级使用技巧](https://blog.csdn.net/qq_34435096/article/details/130729092)
<div class="col-sm mt-3 mt-md-0">
  {% include figure.liquid loading="eager" path="assets/img/posts/mobaxterm.png" class="img-fluid rounded z-depth-1" zoomable=true %}
</div>

{% endtab %}

{% endtabs %}

```shell
# 首次登陆需要yes，随后输入密码
ssh [username]@[ip]
# 例如 ssh ldl@10.106.13.62
```

### 配置存储ip—— ssh config
```shell
# windows 打开终端
cd ~
notepad .\.ssh\config

# 编辑文件 .\.ssh\config 加入：
Host [username]-f3 # 名称，自行配置
HostName 10.106.13.62 #ip 固定
User [username] # 你的id
```

### 配置密钥登录
```shell
# linux环境：
ssh-copy-id -i ~/.ssh/id_rsa.pub [username]@10.106.13.62
```

```shell
# windows环境： 
# 参考：https://cainiaojiaocheng.com/如何在Ubuntu22.04上设置SSH密钥
# 首次使用需要创建密钥： 下方注释行：

# ssh-keygen

# 写入密钥：
cat ~/.ssh/id_rsa.pub | ssh username@remote_host "mkdir -p ~/.ssh && touch ~/.ssh/authorized_keys && chmod -R go= ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

## 文件管理
### scp传输
```shell
scp -r [本机路径] [username]@10.106.13.62:[服务器路径] # 本机->服务器

scp -r [username]@10.106.13.62:[服务器路径] [本机路径] # 服务器->本机
```
### 挂载文件目录
```shell
# 创建软链接
ln -s /mnt/data/kitti-dataset /home/username/path/to/data/directory

# 删除软链接
# 不要加-rf，路径后不要带斜线，如path/to/data/directory/
rm /home/username/path/to/data/directory
```
## 开发
### Tmux

参考：Tmux 使用教程 - 阮一峰的网络日志 (ruanyifeng.com)

使用tmux在ssh断开后继续运行程序_tmux 断开脚本-CSDN博客
``` shell
# Methd 1 
tmux new -s [session_name]
（需要后台的操作）python ./train.py
tmux detach
(下次登录后)
tmux attach -t [session_name]

# Method 2 集体参考链接
tmux 
（需要后台的操作）python ./train.py
ctrl+B 进入控制模式 D 退出
(下次登录后)
tmux a -t 0 
```