+++
title = "Git log の設定"
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

```bash
git lg
git log --oneline --graph
```

## エイリアスの設定

``~/.gitconfig`` に以下を追加すると ``git lg`` または git lga`` が使える

```bash
[alias]
lg = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative
lga = log --graph --all --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative
```
