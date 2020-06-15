---
title: Notes_01|"alias" Knowledge
date: 2020-05-30 10:46:00
abbrlink: 61720
tags:
- Notebook
  - Bash
  - Linux
categories:
  - WSL2
---

  转载自知乎：乃乎。因为 WSL2 Access Windows Proxy 中命令用到了alias方法，就在知乎找到乃乎的分享。
<!-- more -->

alias (别名)，一般用来用一个短的简单的命令来代替长的复杂的命令

## 查看 alias

```bash
Ubuntu@DESKTOP-J51JGI8:~$ alias
alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l='ls -CF'
alias la='ls -A'
alias ll='ls -alF'
alias ls='ls --color=auto'
```

## 临时定义 alias

使用 `ll` 代替 `ls -al`

```bash
alias ll="ls -al"
```

## 持久定义 alias

```bash
vim ~/.bashrc
```

shell 启动的时候会读取配置文件，~/.bashrc 或者 ～/.zshrc。在配置文件中加入 alias 定义的命令就可以在每次启动 shell 后都存在定义好的 alias

```bash
# custom aliases are listed below
$ alias ll="ls -al"
```

## Ref

- [https://www.tecmint.com/create-alias-in-linux/](https://link.zhihu.com/?target=https%3A//www.tecmint.com/create-alias-in-linux/)
