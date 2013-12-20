---
layout: post
title: "httperf でパフォーマンステストを行う"
date: 2012-10-25 01:23
categories: linux
---
httperf は ab (apache bench) と同じようなツールで、サーバの負荷テストに使うことができます。

もうすぐリリースのサーバーに負荷をかける目的で使ってみたのでメモします。


## インストール
```
# Homebrew
$ brew install httperf
# CentOS
$ sudo yum install httperf
```


## URLリストを作る
URLをファイルにまとめておいて、順番にアクセスさせることができます。
実際のユーザのアクセスに近い負荷を作り出すことができますね！

ちなみにファイルのフォーマットはnull文字区切りのテキストファイルです。nginxのアクセスログから作ってみます。

```
$ zcat /path/to/access.log-20121024.gz | awk '{ print $7 }' > urllist.txt
$ tr "\n" "\0" < urllist.txt > wlog.log
```


## 実行
```
$ httperf --hog --server example.com --port 80 --wlog Y,/tmp/wlog.log --rate 200 --num-conn 720000 --num-call 1 --timeout 5
```

`$ man httperf`するとたくさんのオプションの説明が出てきますが、とりあえず使った物は以下の通り。

- hog: 必要な限り多くのTCPポートを使おうとします。指定しないと1024 から 5000 ポートしか使ってくれないのでボトルネックになってしまうことがあるそうです。(正直よくわかってないです。)
- server: ドメインかIPを指定
- port: 接続先ポート
- wlog: アクセス先のpathのリストが入ったファイルを指定する。頭のY/Nはファイル末尾まで終わった後にもう一度頭から繰り返すか。Nにすると、そこで動作終了します。
- rate: 1秒あたりのリクエスト数
- num-conn: 合計接続数。接続数がこの値に達すると動作終了します。
- num-call: 1接続あたりのリクエスト数。今回はセッションを使ったりしないステートレスなサーバなのでシンプルに1にしてみました。
- timeout: タイムアウト（秒）を指定

上記だと、秒間200接続を行い、接続ごとに1つずつpathにアクセスします。合計の接続数が720000になると止まるので、大体1時間くらい負荷をかけ続けることになります。


## 参考URL
- [Na-ga.net » Blog Archive » httperf で Web サーバのベンチマーク - Linux を中心とした忘却メモ](http://na-ga.net/blog/?p=527)
- [httperfによるWebサーバのベンチマーク - (・∀・)イイ!!Memo](http://memo.majide.com/index.php?httperf%A4%CB%A4%E8%A4%EBWeb%A5%B5%A1%BC%A5%D0%A4%CE%A5%D9%A5%F3%A5%C1%A5%DE%A1%BC%A5%AF)
- [httperf - TenForwardの日記 (defiantの日記改め)](http://d.hatena.ne.jp/defiant/20080527/1211873545)
- [Na-ga.net » Blog Archive » httperf man (OUTPUT) - Linux を中心とした忘却メモ](http://na-ga.net/blog/?p=559)
