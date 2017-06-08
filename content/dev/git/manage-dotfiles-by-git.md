+++
title = "Github で dotfiles を管理する"
date = 2017-05-21T06:29:00Z
updated = 2017-05-21T07:45:48Z
draft = true
type = "post"
categories = ["Tools"]
tags = ["Git", "GitHub"]
[author]
	name = "ontheroad_jp"
	uri = ""
+++

```bash
mkdir ~/dotfiles && cd ./dotfiles
mv ~/.bashrc dotfiles
mv ~/.bash_profile dotfiles
mv ~/.bash_history dotfiles

ln -s ~/dotfiles/.bashrc ~/.bashrc
ln -s ~/dotfiles/.bash_profile ~/.bash_profile
ln -s ~/dotfiles/.bash_history ~/.bash_history

cd ~/dotfiles
git init
git add -A
git commit -m "initial commit"
git remote add origin http://github.com/ontheroadjp/dotfiles.git
git push -u origin master
```



