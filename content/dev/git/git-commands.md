+++
title = "Git コマンド覚え書"
date = 2017-05-21T06:29:00Z
updated = 2017-05-21T07:45:48Z
draft = false
tags = ["Git"]
type = "post"
[author]
	name = "ontheroad_jp"
	uri = ""
+++

## ローカルリポジトリの作成

* 初期化して、現在あるファイルを追加して、コミットすればOK

```bash
$ git init
$ git add *
$ git commit -m "initial commit"
$ git push -u origin master # -u で通遺跡ブランチ作成される
```

## リモートリポジトリからプロジェクトをコピー

```bash
$ git clone [リモートリポジトリ名]
$ git clone -b [ブランチ名] [リモートリポジトリ名]
```

### ghq の場合

```bash
$ ghq add [ベンダー名]/[リポジトリ名]
```

## ファイル更新までの基本手順

* ざっくりは以下の様な流れ

### 新規ブランチの作成

1. ``git checkout -b new_branch`` で新規ブランチ作成 & ブランチ変更

### ファイルの編集作業

1. エディタでファイルの編集
2. ``git status`` と ``git diff`` で状態/変更点を確認
3. 必要なファイルをインデックスへ追加（``git add``）  
	→ ``git add`` を取り消す場合は、``git reset``  
	→ ``add`` 済みの ``diff`` は ``diff --cached``
4. コミット（``git commit``）
5. → ``git commit`` を取り消す場合は、``git reset --soft HEAD^``
6. リモートリポジトリへ反映（``git push origin <ブランチ名>``）

### ブランチの派生元（ここでは ``dev`` とする）へ反映

1. ``git checkout dev`` で派生元へ移動
2. ``git merge --no-ff new_branch`` で派生元へ反映
3. ``git push origin dev`` で派生元ブランチをリモートブランチへ反映させる

### 後処理

1. ``git branch -d new_branch`` でブランチ削除



## 初期設定

```bash
$ cd <git 管理したいディレクトリ>
$ git init
```

以上

// git の正当性をチェックする

```bash
$ git fsck
$ git fsck --full
```

// 不要なオブジェクト削除&最適化

```bash
$ git gc
```

## 状態の確認

### インデックスとワーキングツリーの状況

```
$ git status
$ git status -s
```



### コミットログの表示

* ``git log`` は新しいコミットが上に表示される

```bash
# コミットログを表示
$ git log

# コミットログに変更内容を追加して表示
$ git log -p

# コミットログにそれぞれの変更内容（diff）を追加して表示しつつ、最新の3件のみ表示
$ git log -p -3

# コミットの統計情報を見る
$ git log --stat

# コミットログを1行で表示
$ git log --pretty=oneline

# 過去2週間以内のコミットログのみ表示
$ git log --since=2.weeks

# おすすめオプション
```

* ``git log`` //コミットのログが見られる
* ``git reflog`` //HEAD のログが見られる
* ``git reflog origin/branch_name`` //pushのログが見れる
* ログには色々なオプションがあるけど、おすすめは以下のコマンド。

```bash
$ git log --graph --name-status --pretty=format:"%C(red)%h %C(green)%an %Creset%s %C(yellow)%d%Creset"
```

## git diff

### インデックスに add していないモノを表示

* インデックスとワーキングツリーの差分
* 新規に作成されたファイルは Git に track されていないので、新規に作成したファイルはこのコマンドでは表示されない。

	```
	$ git diff
	```

### インデックスに add したモノを表示

* HEADとインデックスの差分
* コミット直前に良く使う

	```bash
	$ git diff --cache
	$ git diff --staged
	```

### 直前のコミットによる変更を表示

* HEAD と HEAD^ の差分
* コミット直後に良く使う

	```bash
	$ git diff HEAD^ HEAD
	```
* ``--stat`` オプションをつけると統計情報のみを表示

	```bash
	$ git diff HEAD^ HEAD --stat
	```


### HEADとワーキングツリーの差分

* ほとんど使うことはない
	
	```bash
	$ git diff HEAD
	```


* diff に --color-words を付けると単語単位で色が付く


## ワーキングツリーの操作

### 編集前の状態に戻す

* 

	```bash
	# 直前のコミットの内容に戻す
	$ git checkout <ファイル名>

	# 任意のコミットの内容に戻す
	$ git checkout <コミットID> <ファイル名>
	```

### ファイルの名前変更

* 

	```bash
	$ git mv [変更前のファイル名] [変更後のファイル名]
	$ git commit -a -m "rename"
	$ git push origin master
	```

#### ファイルの削除（Git 管理対象外にする）

* Git 管理から外す（削除する）
* コミットを実行した時に、ワーキングツリー（PC） からファイルが削除される

	```bash
	$ git rm <ファイル名>
	```

* Git 追跡対象外にしつつ、ワーキングツリー（PC）にファイルを残したい場合は、

	```bash
	$ git rm --cached <ファイル名>
	```

### 特定のモノをインデックスへ追加

* スペース区切りで複数ファイル追加 OK
* ワイルドカード使える　（例）$ git add *.css
* ディレクトリ指定も可能

	```bash
	$ git add ＜ファイル名＞
	```

### 新規作成/変更/削除されたモノを全てインデックスへ追加

* 

	```bash
	$ git add -A
	```

### 変更/削除されたモノのみをインデックスへ追加（新規作成は対象外）

* 

	```
	$ git add -u
	```

#### 新規作成/変更されたモノをインデックスへ追加（削除は対象外）

* 

	```
	git add .
	```

## インデックス(ステージ)の操作

### add したモノを全て元に戻す

* 

	```bash
	$ git reset
	```

### add した特定のモノを元に戻す

* スペース区切りで複数指定可能

	```bash
	$ git reset <ファイル名>
	```

## git push

```bash
# ステージングの内容をリモートレポジトリへ反映
$ git push origin dev

# ステージングの内容をリモートレポジトリへ反映し追跡ブランチを作成
$ git push -u origin dev
```

## git reset

### HEAD とインデックスを<場所>へ移動する（commit と add の取り消し）

* 

	```bash
	$ git reset <場所>
	```

	例）HEAD とインデックスを最終のコミット位置に戻す場合

	```bash
	$ git reset HEAD^
	```

### HEAD のみを<場所>へ移動する（commit のみ取り消し）

* 

	```bash
	$ git reset --soft <場所>
	```

### HEAD, インデックス, ワーキングツリー全てを<場所>へ移動する

* 

	```bash
	$ git reset --hard <場所>
	```

### <場所>の指定方法

* ``HEAD`` 今いるブランチのコミットの先頭
* ``HEAD^`` 最終コミット（直前のコミット）
* ``HEAD~`` 最終コミット（直前のコミット）
* ``@^`` 最終コミット（直前のコミット）
* ``@~`` 最終コミット（直前のコミット）
* ``HEAD~2`` 最終コミットの二つ前

> HEAD^：直前のコミットを意味する。  
> @^、HEAD~も同じ意味。  
> HEAD~{n} ：n個前のコミットを意味する。  
> HEAD^やHEAD~{n}の代わりにコミットのハッシュ値を書いても良い。  
> gitのv1.8.5からは、「HEAD」のエイリアスとして「＠」が用意されている。  
> HEAD^^^とHEAD~3とHEAD~~~とHEAD~{3}と@^^^は同じ意味。  
> --softなので、インデックス・ワーキングツリーはそのまま。  
> 上書きコミットしたいなら、git commit --amendを使うとラク！

出所：[[git reset (--hard/--soft)]ワーキングツリー、インデックス、HEADを使いこなす方法 - Qiita](http://qiita.com/shuntaro_tamura/items/db1aef9cf9d78db50ffe)


### reset を取り消す

* ``ORIG_HEAD reset`` を実行した直前のコミット
* ``ORIG_HEAD`` を使えば ``reset`` 前の状態に戻ることができる
* ``ORIG_HEAD`` は ``reset`` を実行する前の HEAD の位置
* ``FETCH_HEAD`` は ``git fetch`` したコミットの先頭


### reset と revert の違い

* ``reset`` は、履歴が消える（原則、元に戻せない）
* ``revert`` は、履歴が残る

### reset しても git reflog で元に戻す方法

* 

	```bash
	$ git reset --hard HEAD^^ # HEAD^と指定するつもりが間違えた!
	$ git reflog
	f5cb888 HEAD@{0}: head^^: updating HEAD
	b0b8073 HEAD@{1}: merge @{-1}: Merge made by the 'recursive' strategy.
	fe3972d HEAD@{2}: checkout: moving from fix/some-bug to master
	$ git reset --hard HEAD@{1} # reset --hard HEAD^^する前に戻れる
	```

	参考： [いざという時のためのgit reflog - Qiita](http://qiita.com/yaotti/items/e37c707938847aee671b)


## commit（コミットの操作）

### コミットする

* コマンドを実行すると ``$EDITOR`` で指定したエディタが起動するので、最上段の空白行にコメントを入力して、エディタを終了するとコミットされる。

	```bash
	$ git commit
	または
	$ git commit -v # ← diff の内容も表示する
	```

* 環境変数 ``EDITOR`` の指定方法。

	```bash
	export EDITOR=vim
	```


* コミットメッセージをインラインで指定する

	```bash
	$ git commit -m "<コメント>"
	```

### 直前のコミットを上書きしてコミット

* 直前のコミットを作り直す
* `` git commit`` した後に、``git add`` などでインデックスを変更したのち、``git commit --amend`` を実行する

	```bash
	$ git commit --amend
	```

* `` git commit`` した後に、そのコミットのコメントだけを変更する場合は、``git add`` など何もせずに、``git commit --amend`` を実行する

### コミットの取り消し

* 

	```bash
	$ git reset --soft HEAD^	// ワークディレクトリはそのまま
	$ git reset --hard HEAD^	// ワークディレクトリも元に戻す
	```

	* HEAD^	直前
	* HEAD~2	// 2つ前
	* HEAD~3	// 3つ前

### コミットメッセージ

```bash
# 書式
[Prefix] commit message
```

#### Prefix（仕様関連）

|Description|Prefix|
|:---|:---|
|仕様変更|Change|
|バージョンアップ|Upgrade|
|設定追加|Set|
|設定削除|Unset|
|設定の有効化|Enable|
|設定の無効化|Disable|

#### Prefix（機能追加/拡張/向上に関するもの）

|Description|Prefix|
|:---|:---|
|新規機能追加|New (Add)|
|機能拡張|Extend|
|機能向上|Improve|

#### Prefix（メンテナンス関連）

|Description|Prefix|
|:---|:---|
|バグ修正|Fix|
|整理|Clean|
|削除|Remove|
|変更を取り消し|Revert|
|とりあえず保存したい時|Bk|

#### Prefix（その他）

|Description|Prefix|
|:---|:---|
|修正|Modify|
|回避|Avoid|
|更新|Update|
|最適化|Optimize|

#### 例）Bk

* [Bk] WIP
* [Bk] work in progress

#### 例）Clean

* [Clean] cosmetic change

#### 例）Add

* [Add] support for bridgeless SLI with GeForce 8 GPUs.
* [Add] New 3D buttons (新しい３Dボタン (を追加しました))
* [Add] New session management, saving up to 3 queries per request.
* [Add] Provide more helpful features
* [Add] Allow "\n" at the end of line

#### 例）Change

* [Changed] A to B (BできるようにAを変えました)
* [Changed] Minor changes in A (Aにて，小さな変更を行いました)
* [Changed] Major changes in A (Aにて，大きな変更を行いました)

#### 例）Fix

* [Fix] Fixed a performance regression. （パフォーマンスが低下するバグを修正しました）
* [Fix] possible memory leak
* [Fix] possible AAA attack against BBB
* [Fix] stability problems with GeForce 8 GPU. (GeForce 8 GPUで動作が不安定になる問題を修正しました)
* [Fix] an "AAA" bug that was causing the B to C. (BがCしちゃう，いわゆるAAAバグを修正しました)
* [Fix] missing A in B() (関数B内でAが抜けていた不具合を修正しました)
* [Fix] This fixes a regression for ～
* [Fix] This fixes the following bug reported by A. (Aさんが報告してくれた以下の不具合を修正しました)
* [Fix] Various fixes to the install wizard.
* [Fix] Some fixes to make installer work.

### 例）Revert

* [Revert] Reverted SVN rev #396294 due to unwanted regression.(バグがあったので,svnのリビジョン396294に巻き戻しました)
* [Revert] Avoid possible crashes on A　（時々クラッシュするバグを回避するように修正しました）
* [Revert] Avoid segfault on A
* [Revert] Don't crash on A
* [Revert] Removed obsolete username_max_length.
* [Revert] Don't use Location header on IIS
* [Revert] this problem has been fixed この問題は修正済です．
* [Revert] this problem will be fixed shortly この問題は近々修正されます．
* [Revert] We recommend that you upgrade your XXX. アップグレードすることをお勧めします．


## タグ

### タグ追加

```bash
$ git tag -a 1.0.0 -m "<コメント>"

# 過去に遡ってタグをつける
$ git tag 1.0.0 <sha1>
```

### タグ一覧

```bash
$ git tag
```

### 特定のタグにチェックアウト

```bash
$ git checkout refs/tags/2.4.6
```

### 対象タグのコミットを表示

```bash
$ git tag show 1.0.0
```

### タグ削除

```bash
$ git tag -d 1.0.0
```

### タグをリモートへプッシュ

```bash
# タグを指定する
$ git push origin 1.0.0

# push していないタグを全てプッシュ
$ git push origin --tags
```

＜リモートのタグを削除＞

```bash
$ git push origin :refs/tags/v1.0.0
```

## ローカルブランチの操作

```bash
# ローカルブランチの一覧
$ git branch

# リモートとローカルのブランチの一覧
$ git branch -a

# 現在のブランチから新規ブランチを派生させる
$ git branch [branch_name]

# 現在のブランチから新規ローカルブランチを派生して、そのブランチへ移動（リモートブランチがない場合に使う）
$ git checkout -b <ブランチ名>

# 現在のブランチから追跡ブランチと新規ローカルブランチを派生して、そのブランチへ移動（リモートブランチにはあって、ローカルブランチがない場合に使う）
$ git checkout -b ios8 origin/ios8

# 別のブランチに移動
$ git checkout [branch_name]

# ブランチの削除
$ git branch -d [branch_name]

# 現在のブランチ名の変更(変更したいブランチに切り替えた上で実行)
$ git branch -m [new_branch_name]
```

## リモートブランチの操作

```bash
# 追跡ブランチの一覧
$ git branch -r

# 追跡ブランチとローカルのブランチの一覧
$ git branch -a
```


### リモートブランチの削除（ローカルブランチも削除）


```bash
# ローカルブランチを削除
$ git branch -d <削除するブランチ名>

# リモートブランチを削除
$ git push origin :<削除するブランチ名>

# 例: dev ローカル & リモートブランチを削除）
$ git branch -d dev
$ git push origin :dev
```

### リモートブランチの削除（ローカルブランチは残す）

```bash
$ git push --delete origin <削除するブランチ名>
```

## merge（別ブランチの内容を反映）

``master`` 以外のブランチで編集した箇所を ``master`` に反映させる

```bash
# master ブランチへ移動
$ git checkout master

# 差分をマージ
$ git merge [branch_name]

# 差分をマージ（ファーストフォワードマージしない）
$ git merge --no-ff [branch_name]

# リモートリポジトリへ反映
$ git push origin master
```

## リモートレポジトリの設定

```bash
# リモートリポジトリの一覧を表示
git remote -v

# リモートリポジトリ名（ショートネーム）の一覧を表示
$ git remote

# 接続するリモートリポジトリを追加
# 例） git remote add master https://github.com/ontheroadjp/arch-madsonic.git
$ git remote add <shortname> <url>

# 接続するリモートリポジトリを追加(その2)
# 例） git remote add master https://ユーザー名@github.com/ontheroadjp/arch-madsonic.git
$ git remote add <shortname> <url>

# 接続しているリモートリポジトリを解除
# 例） git remote rm origin
$ git remote rm <shortname>

# リモートリポジトリの情報を表示
# 例）git remote show origin
$ git remote show <shortname>

# ローカルの追跡ブランチの情報を更新（リモートブランチの情報と同期）
git remote update -p

# リモートリポジトリの URL 変更
# 例） $ git remote set-url origin https://github.com/ontheroadjp/laravel5_bbc.git
$ git remote set-url [shortname] [url]
```


## stash（今やってる作業を一時退避する）

### スタッシュに保存

```bash
$ git stash
$ git stash save

# メッセージをつけてスタッシュ
$ git stash save "message"

# unstage ファイルを全てスタッシュ
$ git stash -k

# untrackファイルも含めて全てスタッシュ
$ git stash -u
```

### スタッシュの適用

```bash
# 直近のスタッシュを適用して削除
$ git stash pop 

# N 番目のスタッシュを適用して削除
$ git stash pop stash@{N}
```

### スタッシュの一覧

```bash
# スタッシュの一覧
$ git stash list
```

### スタッシュの内容を確認する

```bash
# N番目にスタッシュしたファイルの一覧を表示
$ git stash show stash@{N}

# N番目にスタッシュしたファイルの変更差分を表示
$ git stash show -p stash@{N}
```

### スタッシュの削除

```bash
# 最新のスタッシュを消去
$ git stash drop

# N 番目のスタッシュの消去
$ git stash drop stash@{N}
```

## rebase -i（過去のコミットメッセージを書き換える）

```bash
git rebase -i HEAD~3
```

``pick`` を ``e`` へ変更

``git commit --amend``

``git rebase --continue``

``git push -f origin dev``


## rebase -i（コミットをまとめる）

```
git rebase -i HEAD~4
```

* ``HEAD~2`` で直近のコミット含めて2つのコミットを rebase 対象とする
* ``HEAD~3`` で直近のコミット含めて3つのコミットを rebase 対象とする
* ``HEAD~4`` で直近のコミット含めて4つのコミットを rebase 対象とする
* ``HEAD~5`` で直近のコミット含めて5つのコミットを rebase 対象とする

以下同

rebase 中に訳が分からなくなったら ``git rebase --abort`` で一旦、rebase 処理をキャンセルして初めからやり直す。

rebase を覚えると、何でもかんでも ``git commit -m '[BK] wip`` などでバックアップ代わりにコミットしまくって最後に rebase で一つのコミットにまとめる、などが出来るようになる。

### 例）直近のコミットを含めて4つのコミットを1つにまとめる場合

コミットログが以下の場合、

```bash
b78e6e5  (HEAD -> dev, origin/dev) 2017-02-06 @ontheroad_jp rebase
1cc451a  2017-02-06 @ontheroad_jp [Refactor] change variable names event to item
4e3bdc5  2017-02-06 @ontheroad_jp [New] ajax loading icon for the fetch calendar
aba97c3  2017-02-05 @ontheroad_jp [WIP] table column fix
ca0cae3  2017-02-04 @ontheroad_jp [WIP] Item DnD
70b6ffe  2017-02-03 @ontheroad_jp [WIP]
06b03d9  2017-02-03 @ontheroad_jp [Add] model test
80eec5a  2017-01-23 @ontheroad_jp [Refactor]
111cba5  2017-01-23 @ontheroad_jp [Change] membersApi to store.index.js
```

#### 1. ``git rebase -i HEAD^4`` する

エディタが起動して、以下の通り表示される。古い順に表示されるので最新のコミットが最下段。


```bash
pick aba97c3 [WIP] table column fix
pick 4e3bdc5 [New] ajax loading icon for the fetch calendar
pick 1cc451a [Refactor] change variable names event to item
pick b78e6e5 rebase
```

#### 2. まとめたいコミットの ``pick`` を ``s`` へ変更する

```bash
pick aba97c3 [WIP] table column fix
s 4e3bdc5 [New] ajax loading icon for the fetch calendar
s 1cc451a [Refactor] change variable names event to item
s b78e6e5 rebase
```

``s(squash)`` の意味は、直前のコミットに吸収させる。なのでこの場合は、

最新のコミット(``b78e6e5``) は 2つ前のコミット(``1cc451a``)へ  
2つ前のコミット(``1cc451a``)は 3つ前のコミット(``4e3bdc5``)へ  
3つ前のコミット(``4e3bdc5``)は 4つ前のコミット(``aba97c3 ``)へ

それぞれ吸収され、結果、4つのコミットが一つのコミットになる。``s`` へ変更ごエディタを閉じる。

**注意**

最上段のコミット（対象コミットの中で一番古いコミット：``aba97c3 ``) を ``s`` すると面倒くさいことになる。間違えて ``s`` してエディタを閉じた場合は、``git rebase --abort`` で一旦 rebase をキャンセルして最初からやり直す。

ちなみに最上段のコミット（この場合は4つ前のコミット）を ``s`` した場合、``git rebase -i HEAD^5`` で始める。

エディタを閉じてから ``git log`` するとこんな感じ。最上段の(``109eac6``)が ４つをまとめたコミット


```php
* 109eac6  (HEAD -> dev) 2017-02-05 @ontheroad_jp [WIP] table column fix
| * b78e6e5  (origin/dev) 2017-02-06 @ontheroad_jp rebase
| * 1cc451a  2017-02-06 @ontheroad_jp [Refactor] change variable names event to item
| * 4e3bdc5  2017-02-06 @ontheroad_jp [New] ajax loading icon for the fetch calendar
| * aba97c3  2017-02-05 @ontheroad_jp [WIP] table column fix
|/
* ca0cae3  2017-02-04 @ontheroad_jp [WIP] Item DnD
* 70b6ffe  2017-02-03 @ontheroad_jp [WIP]
* 06b03d9  2017-02-03 @ontheroad_jp [Add] model test
* 80eec5a  2017-01-23 @ontheroad_jp [Refactor]
* 111cba5  2017-01-23 @ontheroad_jp [Change] membersApi to store.index.js
* c026753  2017-01-23 @ontheroad_jp [change] vue-resource to axios
```

まとまったコミット(``109eac6``)のコミットメッセージはまとめ先（この場合は4つ前のコミット）のコミットメッセージになる。

#### 3. ``git push -f`` する

``git push -f origin ＜ブランチ名＞`` すると 4つのコミットがなくなり、まとめたコミットのみが残る。

``git push -f`` 後の git ログ。

```bash
109eac6  (HEAD -> dev, origin/dev) 2017-02-05 @ontheroad_jp [WIP] table column fix
ca0cae3  2017-02-04 @ontheroad_jp [WIP] Item DnD
70b6ffe  2017-02-03 @ontheroad_jp [WIP]
06b03d9  2017-02-03 @ontheroad_jp [Add] model test
80eec5a  2017-01-23 @ontheroad_jp [Refactor]
111cba5  2017-01-23 @ontheroad_jp [Change] membersApi to store.index.js
```

#### 4（必要に応じて） コミットメッセージの変更

``git push -f`` した後に、``git commit -m 'Rebase' --amend`` すると

```bash
* 50600d4  (HEAD -> dev) 2017-02-05 @ontheroad_jp rebase
| * 109eac6  (origin/dev) 2017-02-05 @ontheroad_jp [WIP] table column fix
|/
* ca0cae3  2017-02-04 @ontheroad_jp [WIP] Item DnD
* 70b6ffe  2017-02-03 @ontheroad_jp [WIP]
* 06b03d9  2017-02-03 @ontheroad_jp [Add] model test
* 80eec5a  2017-01-23 @ontheroad_jp [Refactor]
* 111cba5  2017-01-23 @ontheroad_jp [Change] membersApi to store.index.js
```

になるので、再度 ``git push -f`` すると、

```bash
50600d4  (HEAD -> dev, origin/dev) 2017-02-05 @ontheroad_jp rebase
ca0cae3  2017-02-04 @ontheroad_jp [WIP] Item DnD
70b6ffe  2017-02-03 @ontheroad_jp [WIP]
06b03d9  2017-02-03 @ontheroad_jp [Add] model test
80eec5a  2017-01-23 @ontheroad_jp [Refactor]
111cba5  2017-01-23 @ontheroad_jp [Change] membersApi to store.index.js
```

コミットメッセージが変更される。

## .gitignore

* すでに git 管理下にある場合は、``git rm --cached [ファイル名]`` で git 管理外にする必要あり

```bash
# 指定したディレクトリを git 追跡対象から除外
node_modules/
styleguide/

# 指定したファイルを git 追跡対象から除外（ワイルドカード使用可能）
*.txt

# 除外対象から除く（除外しない）場合は先頭に ! ~つける
!readme.txt
```