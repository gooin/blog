---
id: vim-note-chapter1-use-dot
title: vim 笔记 chapter1 '.' 的使用
sidebar_label:  vim 笔记 chapter1 '.' 的使用
slug: /
---

> 这貌似是2016年上学的时候就写的笔记了，整理一下放到网站上，充实下内容hhhh

逛图书馆居然找到一本 Vim 的书，是 Drew Neil 的《Vim 实用技巧》，想想刚开始用 Vim 痛不欲生的日子，（当然现在也是，只不过成了“活着就好”的状态，2333）然后考完试就立刻借来啦，Vim真的是贼好用，当然该开始学的时候确实痛苦，浏览器装个 Vimium 扩展， 简直爽的不要不要的，有了 Vim ，还要鼠标干嘛？ 看了一下这本书有 20 章，打算花 15 天左右慢慢看完，同时用 GVim（windows 的 Vim ）把笔记记下来
<!--more-->


## "."的使用
###  结识 . 命令

> . 命令可以让我们重复上次的修改，他是 Vim 中最为强大的多面手。

这里写几个书上的例子。

- 光标在 *Line* 的位置上时，按下 *x* 会删除一个字符，之后按 *.* 就会重复上次操作，继续删除一个字符。

```javascript
// before
Line one
// after
e one 
```

- *>G* 命令会给当前行到文档末尾添加一个缩进，在第二行使用 *>G* 命令后，按 *j* 跳到下一行，接着再按下 *.* “重复上次修改”，就为每行文本添加缩进。

```javascript
// before
Line one
Line two
Line three
Line four
// after
Line one
        Line two
                Line three
                        Line four
```

### 不要自我重复
> 行尾添加内容这样很简单的操作，Vim 提供了一个专门的命令可以把两步操作合成为一步。

假设有这样的一个代码片段：

```javascript
var vimIsGood = 'Vim da fa is good'
var mdzz = 1
var vim = vimIsGood + mdzz;
```

现在我想要给每行代码的结尾补上分号，一般的步骤是先按下 *$* 将光标移动到行尾，接着按下 *a* 切换到输入模式，然后按下 *;* 输入漏掉的分号，再按下 `Esc` 退出输入模式。

- 减少无关的移动

   a 命令是在当前光标之后添加内容，而 A 是在当前的行尾添加内容。
   对于上面的代码，只需要执行一次 *A;`<Esc>`* 之后，然后就可以愉快的使用命令 *j.* 把每行代码末尾添加分号。

```javascript
 var vimIsGood = 'Vim da fa is good';
 var mdzz = 1;
 var vim = vimIsGood + vim;
```

### 以退为进
假设有这样一段代码

```javascript
 var foo = "("+argument1+","+argument2+")";
```

 看起来有点辣眼睛，因为上面的代码是黏成一片的，如果在 + 号前后都加上一个空格，看起来会舒服不少。
 
 光标在代码开头处，按下 *f+* 将光标移动到 + 处，然后按下 *s* 删除当前的 + 号并切换到输入模式，输入 + ，在之前的 + 号出左右加上空格，然后按下 Esc 退出输入模式。

 接下来的工作就 so easy 了，按下 ; 跳转到上次 f 命令所查找的字符，然后按下 . 来重复上次操作，简单来说就是连着按下 ;. 就可以在后面的 3 个 + 号左右都加上空格了。

 操作完成后看起来一下舒服了不少。

```javascript
 var foo = "(" + argument1 + "," + argument2 + ")";
```

### 查找并手动替换

 > Vim 提供了 :substitute 命令来进行查找替换 :s/target/replacement 方便是方便，但是遇上多义词就有点不合适了，比如：“我在方便的时候想方便一下”这句话，把方便都换为“撒尿”就不合适了。

- 写到这里好像遇到一个尴尬的问题，Vim 好像并不适合中文查找替换。用英文代替吧，毕竟不用中文撸码子。

比如有这样一段话

```html
We'are waiting for content before .... If you are content with this, ... as soon as we have the content..
```
刚开始，把光标移到 “content” 上，然后用 * 命令进行查找，按下 * 后，所有出现这个词的地方都会高亮显示。

当光标位于 “content” 开头时，接着按下 *cw* 删除光标位置到单词结尾间的字符，并进入插入模式，然后输入单词 “copy” 。

Vim 会把我们离开插入模式之前的全部按键操作记录都记下来，所以刚才的过程 *cw*copy`<Esc>` 会被当作一个修改，也就是说，现在的 . 命令会删除当前光标到单词结尾处的字符，并把它修改为“copy”。

接下来按下 n 键，光标会跳到下一个 “content” ,接着再按下 . 键时，会把光标下的单词换为“copy”。

连起来就可以 n. n. 的操作了

```html
We'are waiting for copy before .... If you are copy with this, ... as soon as we have the copy..
```