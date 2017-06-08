+++
title = "Git リポジトリの作成"
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

…or create a new repository on the command line

```bash
echo "# dopecker" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/ontheroadjp/dopecker.git
git push -u origin master
```

…or push an existing repository from the command line

```
git remote add origin https://github.com/ontheroadjp/dopecker.git
git push -u origin master
```
