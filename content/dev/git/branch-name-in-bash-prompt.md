+++
title = "Git ブランチ名を Bash プロンプトに表示する"
date = 2017-05-21T06:29:00Z
updated = 2017-05-21T07:45:48Z
draft = false
type = "post"
categories = ["Tools"]
tags = ["Git", "Bash"]
[author]
	name = "ontheroad_jp"
	uri = ""
slug = "git_branch_name_in_shell_prompt"
+++

## test

```javascript
function fancyAlert(arg) {
  if(arg) {
    $.facebox({div:'#foo'})
  }
}
```

## やりたいこと

* シェル（bash）のプロンプトに git のブランチ名を表示する

## 環境

* Mac OSX Yosemite

## 設定手順

1. ``git-prompt.sh`` と ``git-completion.bash`` の取得 & 設置
2. ``~/.bashrc`` の編集

### (1) git-prompt.sh の取得 & 設置

```bash
mkdir tmp
cd tmp
git clone https://github.com/git/git.git
cp git/contrib/completion/git-prompt.sh /usr/local/etc/bash_completion.d/
cp git/contrib/completion/git-completion.bash /usr/local/etc/bash_completion.d/
```

### (2) .bashrc の編集

* ``.bashrc`` に以下を追加する

```bash
source /usr/local/etc/bash_completion.d/git-prompt.sh
source /usr/local/etc/bash_completion.d/git-completion.bash
GIT_PS1_SHOWDIRTYSTATE=true
export PS1='[\[\033[32m\]@\h\[\033[00m\]:\[\033[34m\]\w\[\033[31m\]$(__git_ps1)\[\033[00m\]]\'
```

* ``export PS1=`` の設定はお好みで
* ``$(__git_ps1)`` が git のブランチ名
* ``.bashrc`` の編集が終わったら ``source .bashrc`` を忘れずに

## 表示イメージ

```bash
[@PCName: ~/Vagrant/LAMP/Peach (master *)]
```

## シェル起動時に .bashrc が読み込まれない場合

* ``.bash_profile`` に以下を追加する

```bash
if [ -f ~/.bashrc ] ; then
. ~/.bashrc
fi
```

----------------------------------------------


## bash-completion のインストール

```bash
# ダウンロード
wget http://bash-completion.alioth.debian.org/files/bash-completion-1.2.tar.bz2

# インストール
tar xvjf bash-completion-1.2.tar.bz2
cd bash-completion-1.2
./configure
make
sudo make install
```

``/usr/local/etc`` に ``bash_completion.d`` としてインストールされる

## git-prompt.sh のインストール

```bash
wget https://raw.githubusercontent.com/git/git/master/contrib/completion/git-prompt.sh -O ~/.git-prompt.sh
sudo mv /usr/local/etc/bash_completion.d/git-prompt.sh
```

```bash
wget https://raw.githubusercontent.com/git/git/master/contrib/completion/git-prompt.sh
```


* [git-prompt.sh ダウンロードページ](http://git-prompt.sh/)

## git-completion.bash のインストール

```bash
wget 'http://git.kernel.org/?p=git/git.git;a=blob_plain;f=contrib/completion/git-completion.bash;hb=HEAD' -O git-completion.bash
sudo mv git-completion.bash /usr/local/etc/bash_completion.d/git
```

## ~/.bashrc の編集

```
# プロンプトに git のブランチ名を表示する
source /usr/local/etc/bash_completion.d/git-prompt.sh
source /usr/local/etc/bash_completion.d/git-completion.bash
GIT_PS1_SHOWDIRTYSTATE=true
export PS1='[\[\033[32m\]@\h\[\033[00m\]:\[\033[34m\]\w\[\033[31m\]$(__git_ps1)\[\033[00m\]]\'
```

## ~/.bashrc の読み込み

```bash
source .bashrc
```

## エラーが出た場合。

インストールが上手くいってないと、以下のエラーが出る。

```bash
-bash: __git_ps1: command not found
```

``/usr/local/etc/bash_completion.d`` 以下に、``git-prompt.sh`` と ``git-completion.bash`` があるか確認する。
