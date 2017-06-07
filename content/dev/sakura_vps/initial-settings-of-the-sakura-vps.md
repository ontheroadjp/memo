+++
title = "さくら VPS 初期設定"
date = 2017-04-19T19:01:00Z
type = "post"
draft = false
tags = ["さくら VPS"]
+++

# さくら VPS の基本設定


## ホスト名の設定

```bash
$ vim /etc/sysconfig/network

#HOSTNAME=localhost.localdomain
HOSTNAME=NOBITA.localdomain
```

## VNC コンソールのキー配列の設定

MacBookPro US 配列からさくらVPS の CentOS6.7（標準OS）へ、さくらのVNC コンソール経由で接続すると、キー入力が正しく行えないので、キーボード設定を正しく設定する必要がある。

キーボード設定は ``さくらVPS の設定`` と ``CentOS の設定`` を行う必要あり。

### 1. さくらVPS の設定

* [VPSコントロールパネル](https://secure.sakura.ad.jp/vps/#/login)にログイン後、画面右上の ``設定`` メニューから ``サーバー情報編集`` を選択して ``VNC コンソールキー配列`` から ``en-us`` を選択する。

* CentOS を再起動すると変更が有効になる。

参考：[さくらのサポート情報:キー配列の変更方法](https://help.sakura.ad.jp/app/answers/detail/a_id/2406/related/1)

### 2. CentOS の設定

* ``$ loadkeys us`` コマンドで US 配列になる（ちゃんと入力できるようになる）けど、CentOS を再起動するたびに設定をしなければならない。
* なので、とりあえず ``$ loadkeys us`` でちゃんと入力できるようにしてから、 ``/etc/sysconfig/keyboard`` の中を、

	```
	KEYTABLE="us"
	MODEL="pc105+inet"
	LAYOUT="us"
	KEYBOARDTYPE="pc"
	```
	
	に変更する。

* ``$ loadkeys us`` する前に、``vi`` を起動すると ``:`` が入力できなくて、強制再起動する羽目になる。

## SSH 接続の設定

### 1. 秘密鍵と公開鍵ペアの生成

```bash
$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/username/.ssh/id_rsa):  ← 作成される場所。問題なければEnter。
Enter passphrase (empty for no passphrase):  ← パスフレーズの入力。省略する場合はそのままEnter。
Enter same passphrase again:  ← パスフレーズの再入力。省略した場合はそのままEnter。
```

上記実行すると、``~/.ssh/`` に秘密鍵（``id_rsa``）と公開鍵（``id_rsa.pub``）が生成される。

### 2. さくらVPS へ公開鍵を配置

SSH 接続したいユーザーのホームディレクトリに ``.ssh/`` を作成し、``authorized_keys`` という名前で保存する。

``~/.ssh/authorized_keys``

### 3. SSH 接続元の設定

``~/.ssh/config`` に以下の内容を記入して保存すると ``$ ssh <ホスト名>`` で接続できる。

設定例

```
$cat ~/.ssh/config

Host sakuraroot
	HostName xxx.xxx.xxx.xxx
	Port 22
	User root
 
Host sakura
	HostName xxx.xxx.xxx.xxx
	Port 10022
	User nobita
```

SSH 接続

```
$ ssh sakura
```

1. コンソール VPS でログイン

	* ユーザー名： root
	* パスワード： インストール時に設定したパスワード

2. root で SSH 接続設定

	* ``~/.ssh/config`` を作成する
	* 接続時に root パスワード聞かれる（パスワード認証）

	```bash
	$vim ~/.ssh/config
	
	Host sakuraroot
		HostName xxx.xxx.xxx.xxx
		Port 22
		User root
	```

3. ``$ ssh sakuraroot`` で SSH 接続確認
4. Chef solo のインストール

	* 接続時に root パスワード聞かれる（パスワード認証）

	```bash
	$ knife solo prepare sakuraroot
	```

5. Chef solo の実行

	* 接続時に root パスワード聞かれる（パスワード認証）

	```bash
	$ knife solo cook sakuraroot
	```

	* Chef solo 実行後は ``$ ssh sakuraroot`` では SSH 接続ができなくなる（SSH ポートが 22番から 10022番へ変更となるため）

6. 一般ユーザーで SSH 接続

	* ``~/.ssh/config`` に以下を追記

	```bash
	$vim ~/.ssh/config
	
	Host sakura
		HostName xxx.xxx.xxx.xxx
		Port 10022
		User nobita
	```

	* 接続できたら ``$ su -`` で root 権限が得られるか確認
	* ``sudo`` も確認。例）``$ sudo iptables -L``


