+++
title = "過去の コミットメッセージを書き換える"
date = 2017-05-21T06:29:00Z
updated = 2017-05-21T07:45:48Z
draft = true
type = "post"
categories = ["Tools"]
tags = ["Git"]
[author]
	name = "ontheroad_jp"
	uri = ""
+++

```bash
# 修正したいコミットの一つ前のコミットハッシュをコピー
git log --graph --date-order --all --pretty=format:'%h %Cred%d %Cgreen%ad %Cblue%cn %Creset%s'

git rebase -i ＜修正したいコミットの一つ前のコミットハッシュ＞
```

vim が起動したら・・・

コミットメッセージを変更したいコミットの ``pick`` を ``r``（または ``reword``） に変更して vim を終了するとコミットメッセージ修正画面に変わるので、コミットメッセージを修正して vim を終了させる


