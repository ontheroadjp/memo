+++
title = "Git で バックアップ"
date = 2017-05-21T06:29:00Z
updated = 2017-05-21T07:45:48Z
draft = false
type = "post"
categories = ["Tools"]
tags = ["Git"]
[author]
	name = "ontheroad_jp"
	uri = ""
+++

## やりたいこと

* 今まで作業ディレクトリを bk_yyyy_mm_dd_ディレクトリ名.zip みたいにコピー&圧縮してバックアップを取っていたものを git を使ってやってみたい。
* 中途半端な状態であろうがなんだろうがとにかく保存（バックアップ）したい。

## 運用方法

* バックアップ用のブランチをつくる。
* 普段はバックアップ用のブランチで作業。
* バックアップする時は中途半端だろうがなんだろうが add -A して commit。
* いちいち add -A して commit は面倒くさいので シェルスクリプトで。
* コミット時のメモに日時を付け加えて、いつ時点のバックアップかをわかるように。
* clone で定期実行してもいいけど今はしていない。

## 参考

* [gitで定期的にファイルをバックアップする](http://qiita.com/irxground/items/80dc6432e7d9d2b8b2a9)

## 手順

1. バックアップ用のブランチを作成してバックアップブランチへ移動する

	```bash
	$ git checkout -b bk
	```

2. シェルスクリプトを作成して適当な名前で保存（``backup.sh`` など）

	```bash
	#!/bin/bash
	set -e
	content_dir="バックアップ対象のディレクトリ"

	set -x
	cd "$content_dir"
	git add -A
	git commit -m "backup at $(date "+%Y-%m-%d %T")" || true
	git push -f origin bk
	```

* バックアップ対象のディレクトリはフルパスで指定  
	例：

	```bash
	content_dir="$HOME/Vagrant/LAMP/www/hogehoge"
	```

* バックアップ対象のディレクトリは当たり前だけど git 管理していることが前提

## バックアップの実行

1. シェルを実行するだけ。実行すればありのままをコミットしてくれる

	```bash
	sh backup.sh
	```






