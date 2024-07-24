---
original: true
title: homebrew的环境变量
description: 通过homebrew安装的软件是否需要设置环境变量
date: 2023-03-23
slug: 
image: 
tags:
    - homebrew
categories:
    - 
---

homewbrew在macOS Intel中的安装路径是 `/usr/local`，homewbrew在Apple Silicon中的安装路径是 `/opt/homebrew`。这里以M1电脑为例，brew安装的软件都存放在 `/opt/homebrew/Cellar`。brew 会为每个软件的bin目录创建一个软连接到`/opt/homebrew/bin` 里面。比如 我们通过`brew install go`

，那么在 `/opt/homebrew/bin` 目录下就会有一个 go 软连接，这个go 软件对应的原身就是 `/opt/homebrew/Cellar/go/1.20.1/libexec/bin/go`。因为我们安装homebrew的时候会配置环境变量。会把 `/opt/homebrew/bin` 配置到环境变量里面，这样间接就配置了所有通过brew安装的软件的环境变量。

