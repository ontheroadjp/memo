+++
title = "Git push で 403 エラー"
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

``git push`` した時に以下のような 403 エラーが出た。

```bash
error: The requested URL returned error: 403 Forbidden while accessing https://xxxxx
```

設定を確認してみる。

```bash
$ git config --list

--省略--
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
remote.origin.url=https://github.com/username/repository.git
--省略--
```

``remote.origin.url`` に github のアカウント名を追加してみる。（以下の ``<username>@`` の場所）

```bash
$ git remote set-url origin https://<username>@github.com/username/repository.git
```

っでもう一度 ``git push``

```bash
$ git push origin master
password:
```

パスワードを聞かれるので github のパスワードを入力したら無事に ``git push`` できた。

```
Counting objects: 22, done.
Delta compression using up to 3 threads.
Compressing objects: 100% (11/11), done.
Writing objects: 100% (12/12), 1.21 KiB, done.
Total 12 (delta 4), reused 0 (delta 0)
To https://<username>@github.com/username/registory.git
   9c7e824..36cf324  master -> master
```

