+++
title = "Git ブランチの操作"
date = 2017-05-21T06:29:00Z
updated = 2017-05-21T07:45:48Z
draft = false
tags = ["Git"]
type = "post"
[author]
	name = "ontheroad_jp"
	uri = ""
+++

### 基本コマンド

まずはローカルリポジトリでブランチを新規作成したり削除したりするコマンド

#### ブランチ一覧の表示

```bash
git branch
```

#### 新規ブランチの作成

```bash
git branch ＜作成するブランチ名＞
```

#### ブランチの削除

```bash
git branch -d ＜削除するブランチ名＞
```

#### ブランチ名の変更

```bash
git branch -m ＜変更後のブランチ名＞
```

### リモートリポジトリのブランチとの連動

次にリモートブランチとの連動。はじめはイマイチローカルブランチとリモートブランチの関係を理解しにくいけど、追跡ブランチの概念を理解するとわかりやすい。そのあたりは「<a href="http://www.kaeruspoon.net/articles/1078" target="_bkank">Gitのリモートブランチと追跡ブランチは違うよ</a>」の記事が参考になるかも。 


#### 追跡ブランチ一覧の表示

```bash
git branch -r
```

* このオブション（-r）は、リモートブランチ一覧の表示じゃない
* 参考： <a href="http://www.kaeruspoon.net/articles/1078" target="_bkank">Gitのリモートブランチと追跡ブランチは違うよ</a>

#### 追跡ブランチとリモートブランチの紐付け一覧の表示

```bash
git branch -vv
```

#### 表示例

```bash
git branch -vv
  bk     a49334e [origin/bk] backup at 2015-08-12 11:06:24
* dev    802a795 [origin/dev: gone] add gulp-connect-php
  master 5e87124 [origin/master] modified: centering swap btn
```

* * がついているのが現在いるブランチ
* [] の中が紐付いているリモートブランチ
* [] の中に 「:gone」が付いているのは、紐付け解除されている状態（以前は紐付いていた）


#### 特定のリモートブランチとの紐付け

```bash
git branch -u ＜リモートリポジトリ名＞/＜紐付けるリモートブランチ名＞
```

#### リモートブランチとの紐付けを解除する

```bash
git branch -r -d ＜リモートリポジトリ名＞/＜解除するリモートブランチ名＞
```


### 各ブランチの最終コミットの一覧の表示

```bash
git branch -v
```


### ブランチ紐付けの例

```bash
[src/]git branch -vv
* bk     a49334e backup at 2015-08-12 11:06:24
  master 5e87124 [origin/master] modified: centering swap btn
```

```bash
[src/]git branch -u origin/bk
Branch bk set up to track remote branch bk from origin.
```

```bash
[src/]git branch -vv
* bk     a49334e [origin/bk] backup at 2015-08-12 11:06:24
  master 5e87124 [origin/master] modified: centering swap btn
```


