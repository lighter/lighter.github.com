---
layout: post
title: "Install nvm on mac"
date: 2015-03-25 05:47:08 +0800
comments: true
tags: 
- nvm
---
`nvm`可以讓你安裝多個`node`版本，並且切換現在使用的版本，其實這跟`ruby`的`rvm`很像。

首先更新`brew`。

```
$ brew update
$ brew upgrade
```

然後安裝`nvm`。

`$ brew install nvm`

最後需要將下面這行加入到`.profile`, `.bashrc` 或 `.zshrc`	，這樣才可以執行`rvm`，因為我是使用`zsh`，所以我加到`.zshrc`。

`$ echo "source $(brew --prefix nvm)/nvm.sh" >> ~/.zshrc`

## 安裝 node & npm

可以使用下面這個指令列出所有的`node`版本

`$ nvm ls-remote`

接著我想裝`v0.12.1`的版本

`$ nvm install -s v0.12.1`

安裝完成後使用下面這指令可以看到安裝好的版本

```
$ node -v
$ npm -v
```

下面這行指令可以讓你每次預設自動使用的版本。

`$ nvm alias default 0.12.1`

如果要切換版本可以使用。

`$nvm use VERSION`