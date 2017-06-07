+++
title = "Bash 便利コマンド"
date = 2017-04-19T19:01:00Z
type = "post"
draft = true
tags = ["Bash"]
slug = "bash-useful-commands"
+++

## パイプで渡したコマンドの終了ステータスが知りたい

通常パイプでコマンドを渡すと、``$?`` では最後に実行したコマンドの終了ステータスしか見ることができない。

パイプで渡したコマンドの終了ステータスが見たいときは、``PIPESTATUS`` を見ればよい

```bash
$ cat test.sh | cat | cat | cat | cat
...
$ echo ${PIPESTATUS[@]}
0 0 0 0 0
```

出処: [検索ではあんまり出ないbashの便利技 - Qiita](http://qiita.com/rsooo/items/ef1d036bcc7282a66d7d)

## ブレース展開

```
$ cp testfile{,_bk} #cp testfile testfile_bk と等価
$ ls testfile*
testfile  testfile_bk
```

出処: [検索ではあんまり出ないbashの便利技 - Qiita](http://qiita.com/rsooo/items/ef1d036bcc7282a66d7d)

