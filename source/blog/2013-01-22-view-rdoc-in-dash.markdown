---
layout: post
title: "Dash で gem のドキュメントを参照する"
date: 2013-01-22 18:17
tags: ruby
---

<blockquote class="twitter-tweet" data-in-reply-to="293602772425777154" lang="ja"><p>@<a href="https://twitter.com/satococoa">satococoa</a> やっぱウェブみにいくんですね、了解です。この辺も Dash とかで見れるようにしたいな</p>&mdash; Naoya Itoさん (@naoya_ito) <a href="https://twitter.com/naoya_ito/status/293602967179911168" data-datetime="2013-01-22T06:16:16+00:00">1月 22, 2013</a></blockquote>

こんな話から、インストールした gem のドキュメントが見られる風な Docset が Dash にあったのを思い出してちょっと調べてみました。

以下の手順に沿ってちょこちょこっと設定をすると、RDoc で生成された gem のドキュメントが見られました。

### 設定方法

1. Preferences… -> Downloads から "Ruby Installed Gems" というdocsetをインストール
1. Preferences… -> Docsets に Ruby Gems という docset があるので、その一番右にあるギヤのボタンからrdocが置かれているパスを設定する。`gem env gempath` というコマンドで rdoc の置かれるパスがわかります。
1. "index" ボタンを押すと Dash で使えるようになります。

gem をインストールするときには .gemrc なりオプションなりで `--no-ri --no-rdoc` を指定している人が多いと思います。以下のコマンドで任意の gem の rdoc を生成することが出来ます。

```
$ gem rdoc --rdoc bubble-wrap
```


### 結果

こんな感じです。

![Rdoc in Dash](/images/201301/rdoc-in-dash.png)


### ついでに
横道にそれますが gem に限るのでしたら `yard` もおすすめです。

```
$ gem install yard
$ yard server --gems
```

`gem rdoc` コマンドで rdoc を生成しておく必要もありません。
