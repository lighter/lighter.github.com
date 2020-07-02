---
layout: post
title: "NodeJS 安裝"
date: 2013-03-31 08:31
comments: true
tags: 
- NodeJS
---

系統：Mountain Lion

[Homebrew](http://mxcl.github.com/homebrew/index_zh-tw.html)是一個Mac OS套件管理工具

因為Mac上預設都有安裝Ruby了，所以可以直接用以下的指令安裝

```
> ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"
```

1. 安裝node
```
> brew update
> sudo brew install node
```

2. 安裝npm
```
> sudo npm install express -g
```

3. 修改npm PATH，開啟`~/.bash_profile`，加入以下的代碼
```
export PATH=/usr/local/bin:$PATH:/usr/local/share/npm/bin
```

4. 接著就可以使用`express`指令來建立專案了
```
express new project
```
