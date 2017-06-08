+++
title = "sed いろいろ"
date = 2017-04-19T19:01:00Z
draft = false
type = "post"
categories = ["Linux"]
tags = ["Shell Script"]
+++

# sed いろいろ

## コメントアウトを外す

例） /etc/nginx/conf.d/default.conf

```bash
sed -i -e "s/\(.*\)#\(location\)/\1\2/" ${out}
sed -i -e "s/\(.*\)#\(    root\)/\1\2/" ${out}
sed -i -e "s/\(.*\)#\(    fastcgi_pass\)/\1\2/" ${out}
sed -i -e "s/\(.*\)#\(    fastcgi_index\)/\1\2/" ${out}
sed -i -e "s/\(.*\)#\(    fastcgi_param\)/\1\2/" ${out}
sed -i -e "s/\(.*\)#\(    include\)/\1\2/" ${out}
```

* () でくくると、``\1``, ``\2`` で参照できる
* 上記の場合、1つ目の括弧は ``#`` までの文字列で、2つ目の括弧は ``#`` 以降の文字列 
