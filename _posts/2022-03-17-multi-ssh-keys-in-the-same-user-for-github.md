---
layout: post
title: "Linux 同一个账户配置多个 ssh key 用于与不同 GitHub 账户连接"
author: "cya"
header-style: text
tags:
  - GitHub
---

配置实验室服务器访问自己的 GitHub 账号，想把已有的 `~/.ssh/id_rsa` 添加到自己账户的 ssh keys 中，但是报错 Key is already in use。原因是 GitHub 不允许一个 ssh key 给多个账户使用。

因此我需要在服务器上生成一个新的 ssh key 给自己的账户用，但是在 `git clone` 时会默认用 `~/.ssh/id_rsa` 来与 github.com 进行身份验证，因此要配置一下。

在文件 `~/.ssh/config` 中添加：

```
Host yuangcao.github.com
 Hostname github.com
 AddKeysToAgent yes
 IdentityFile /home/kvgroup/cya/.ssh/id_rsa_cya
```

运行 `ssh -T git@yuangcao.github.com`，若输出：

```
Hi yuangcao! You've successfully authenticated, but GitHub does not provide shell access.
```

说明配置成功。

之后在 clone 我的私有仓库时使用：

```bash
git clone git@yuangcao.github.com:yuangcao/repo.git
```

