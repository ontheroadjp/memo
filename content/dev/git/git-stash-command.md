+++
title = "Git stash"
date = 2017-05-21T06:29:00Z
updated = 2017-05-21T07:45:48Z
draft = false
categories = ["Git"]
tags = ["Git"]
type = "post"
[author]
	name = "ontheroad_jp"
	uri = ""
+++

gitは、とにかくトピックブランチを作成して作業する。だいたい機能追加とかバグ修正とかの単位でブランチを作って作業します。（ちゃんとやってますよね？）

なので、作業の途中で別の修正を優先してお願いっ！なんて言われたときは、別のブランチに切り替えて作業をする必要がでてくる。そんな時に変更を一時的に退避しておくことのできる機能、それが ``stash`` である。

では、早速使い方

## 使い方

### stash へ退避

まだ ``commit ``していない状態の変更ファイル（``add`` してる or ``add`` していない)が存在する状況で、次のコマンドを実行すると変更ファイルを退避することができる。

```bash
$ git stash save
```

※ saveは、省略することもできる。

これでファイルの退避完了！``git status``とか見てみると変更状態であったファイルがなくなっている。この状態なら安心してブランチを切り替えることもできる。めでたしめでたし。

と、退避だけならこれで終わりだけど、次につかうときに変更ファイルを取り出したい。

### stash の中身（退避しているモノ）の確認

まずは、いまどんな変更を退避しているかを確認
2つの変更を保存している状態（2回stashを行った状態）

```bash
$ git stash list
stash@{0}: WIP on sub: a0d2f1b add fourth line
stash@{1}: WIP on sub: 1a61919 add second line
```

``<stash名>: WIP on <stashを行ったブランチ名>: <ハッシュ> <コミットコメント>``

※ハッシュ、コミットコメントは ``stash`` を行った時の HEAD のもの

次のようにすると、さらに変更内容もみれるので便利！（``git log`` のオプションが使える）

```bash
$ git stash list -p
```

さらに各変更について詳しく見たい場合、変更内容に含まれるファイルの一覧が見れる。

```bash
$ git stash show <stash名>
```

### stash から取り込む

復活させたい stash 名がわかったら次のコマンドで取り出すことができる。

```bash
$ git stash apply stash@{0}
```

``stash apply`` で変更を復活した場合は、stash リストのなかに復活済みの変更が残されているこれを削除するには、次のコマンドを使用する。

### stash の削除

```bash
$ git stash drop <消したいstash名>
$ git stash drop stash@{0} # ex.
```

### stash からの取り込みと削除の同時実行

変更の復活と、削除を同時に行う。

```bash
$ git stash pop stash@{0}
```

``stash`` が溜まってくると、なんの変更だったか、どれが必要な変更だったかわからなくなるのでこまめに消したり管理するのが大切ですよー

## 応用

``git apply`` で、変更を復活して適用したんだけど、やっぱやめとこうってときにこんなこんなやりかたで取り消せます。前半は ``git stash show -p`` でパッチ形式で出力、それを ``git apply -R`` でパッチを逆に適用しています。

```bash
$ git stash show <適用したstash名> -p | git apply -R
```