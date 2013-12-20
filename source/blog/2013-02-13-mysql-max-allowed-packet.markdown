---
layout: post
title: "MySQL の max_allowed_packet を設定"
date: 2013-02-13 00:05
tags: mysql
---
ActiveRecord で `Mysql2::Error: MySQL server has gone away` と言われて困ったのです。

さっぱり原因がわからずに同僚の [@DianthuDia](https://twitter.com/dianthudia) 先生に聞いてみたところ、どうやら大きなデータを MEDIUMTEXT 型のカラムに保存しようとしたときに、以下の MySQL の設定にひっかかって失敗していたらしいです。

```
> show variables like 'max_allowed_packet';
+--------------------+---------+
| Variable_name      | Value   |
+--------------------+---------+
| max_allowed_packet | 1048576 |
+--------------------+---------+
1 row in set (0.01 sec)
```

`max_allowed_packet` は mysql サーバがクライアントから受け付けることの出来るパケット量の設定です。


```
# brew で入れた mysql の場合
$ cp /usr/local/Cellar/mysql/5.5.29/support-files/my-small.cnf /usr/local/etc/my.cnf
$ vim /usr/local/etc/my.cnf
# [mysqld] 中に以下を追加
[mysqld]
max_allowed_packet=16M
```

MySQLを再起動して設定完了です。

再起動無しで設定するには `GLOBAL VARIABLES` を設定します。

```
$ mysql -uroot -p
> set global max_allowed_packet = 16 * 1024 * 1024;
> show global variables like 'max_allowed_packet';    
+--------------------+----------+
| Variable_name      | Value    |
+--------------------+----------+
| max_allowed_packet | 16777216 |
+--------------------+----------+
1 row in set (0.01 sec)
```

大丈夫になったっぽいです。ありがとうございました。  
詳しい人が社内にいると安心感がすごい。
