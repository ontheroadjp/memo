+++
title = "部分文字列を取り出す"
date = 2017-04-19T19:01:00Z
draft = true
type = "post"
categories = ["Linux"]
tags = ["Shell Script"]
index = false
+++

## パラメータ展開を利用して部分文字列を取得する

オフセットと長さを指定して文字列を取得する

```bash
指定方法
${パラメータ:オフセット:長さ}
```

```bash
部分文字列抽出
#!/bin/bash

HOGE="abcdef"

# オフセット位置から長さ分を取得
echo ${HOGE:0:2}
# -> ab

echo ${HOGE:2:2}
# -> cd

echo ${HOGE:4:2}
# -> ef

# 長さを省略した場合はオフセットから最後まで出力
echo ${HOGE:2}
# -> cdef

# 長さにマイナスを指定した場合は最後からマイナス分引いた位置までの長さになる
echo ${HOGE:0:-2}
# -> abcd

# オフセットの位置にマイナスを指定した場合は文法として別のパラメータ展開になる(デフォルト値の指定)
# 指定した変数が空文字列の場合は右に指定した文字が入る
echo ${HOGE:-2}
# -> abcdef
HOGE=
echo ${HOGE:-2}
# -> 2
```

## %, %% 右端からの(最短の|最長の)パターン一致までを除外

```bash
右端からのパターン一致除外
#!/bin/bash

HOGE=hoge.tar.bz2

echo ${HOGE}
# -> hoge.tar.bz2

# 最短除外
echo ${HOGE%.*}
# -> hoge.tar

# 最長除外
echo ${HOGE%%.*}
# -> hoge
```

## #, ## 左端からの(最短の|最長の)パターン一致までを除外

```bash
左端からのパターン一致除外
#!/bin/bash

HOGE=/home/user/hoge

echo ${HOGE}
# -> /home/user/hoge

# 最短除外
echo ${HOGE#*/}
# -> home/user/hoge

# 最長除外
echo ${HOGE##*/}
# -> hoge
```
