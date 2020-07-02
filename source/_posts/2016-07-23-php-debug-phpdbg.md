title: php debug phpdbg
date: 2016-07-23 23:19:43
categories:
- coding
tags:
- php
- phpdbg
---

簡單記錄一下`phpdbg`的使用方式。

建立一個簡單sample code

```
<?php

function test()
{
  $a = 0;

  while ($a < 10)
  {
    $a++;
    echo $a;
  }
}

test();
```

先執行`phpdbg`與想要breakpoint的code

```
$ phpdbg test.php
```

![](/images/phpdbg/phpdbg_1.png)

如果有不知道的指令可以使用`help`

設定斷點在指定的行數

```
>prompt b 9 # 斷在 $a++;
```
![](/images/phpdbg/phpdbg_2.png)

列出你要看的function

```
>prompt l f test # list function test
```

![](/images/phpdbg/phpdbg_3.png)

列出你設定的breakpoint

```
>prompt info break
```

![](/images/phpdbg/phpdbg_4.png)

刪除break point

```
>prompt break del 1 # 1代表你的第幾的break point
```

![](/images/phpdbg/phpdbg_5.png)

執行

```
>prompt run
```

觀察變數

```
>prompt info vars
```

![](/images/phpdbg/phpdbg_6.png)

執行php

```
>prompt ev $test = 'ttt'
ttt

>prompt ev var_dump($test);
string(3) "ttt"
```

![](/images/phpdbg/phpdbg_7.png)

單步執行

```
>prompt s
```

![](/images/phpdbg/phpdbg_8.png)

結束執行

```
>prompt finish
```

![](/images/phpdbg/phpdbg_9.png)

結束`phpdbg`

```
>prompt q # quit
```
## 參考

1. [PHP 调试利器之 PHPDBG](http://blog.jobbole.com/97714/)
2. [Phpdbg debugger: quickstart guide and practical uses](http://samtuke.com/2014/07/phpdbg-debugger-quickstart-guide-and-practical-uses/)
3. [Intro to PHP DBG](https://www.youtube.com/watch?v=7WoCd-E0ptQ)
4. [phpdbg for fun and profit](https://lawngnome.github.io/phpdbg-for-fun-and-profit/#/title)
