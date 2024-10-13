---
title: fabric introduce
date: 2024-10-13 15:32:46
tags: 
- ai
- fabric
---
[Fabric](https://github.com/danielmiessler/fabric)是什麼?

他是由Daniel Meissler創建的一個開源AI prompt。這個工具的目的是更有效地使用AI服務。通過一系列定義好的pattern來改提升AI的回答質量。\[2\]

目前我很喜歡用在兩個地方：

1. youtube影片的快速幫我摘要內容。

2. git diff的差異快速幫我產生commit。

# Youtube 摘要

大家可以參考原作者的介紹\[3\]，原作喜歡動手寫筆記，而他寫了一個`extract_wisdom`的pattern。來幫助他可以快速地做筆記，有興趣可以直接去[extract_wisdom](https://github.com/danielmiessler/fabric/blob/main/patterns/extract_wisdom/system.md)裡面看看他怎麼寫的prompt。而指令可以這用使用，當中的`-s`代表stream，不用等待ai最終全部一次反饋，而是透過流水的方式一直輸出回應。`-p`代表你要使用的 pattern，`-g`代表你最終要輸出的語言是什麼，但是後面更新的版本似乎可以一開始就設定好你預設的語言，所以可以不用額外下這個參數了。

```
$ fabric -y=URL -sp extract_wisdom -g=zh_TW
```

# Git diff 差異摘要

而我最近用很多的是幫我寫git commit，我的指令是這樣下的：

```
$ git diff --staged | fabric -sp summarize_git_diff
```

這樣會根據目前我加到stage的檔案，去寫產出這次的差異commit，然後自己再自行確認一下。大大省下我很多時間呢！

你也可以在終端機來問問題，指令可以這樣下：

```
$ echo "台灣最高的山是什麼山?" | fabric -sp ai
```

只是這種一問一答的方式我自己還是偏好使用有GUI的介面問答。如果只是想問簡單的問題，你也可以試試。

### ref

\[1\]. [https://github.com/danielmiessler/fabric](https://github.com/danielmiessler/fabric)

\[2\]. [https://www.youtube.com/watch?v=0qlyCPvmH2Y](https://www.youtube.com/watch?v=0qlyCPvmH2Y)

\[3\]. [https://youtu.be/wPEyyigh10g?si=0T9GUk58AUgwZFSC&t=544](https://youtu.be/wPEyyigh10g?si=0T9GUk58AUgwZFSC&t=544)