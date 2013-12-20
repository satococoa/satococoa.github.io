---
layout: post
title: "Hello Octopress"
date: 2012-06-04 00:45
comments: true
categories: 日記
---
遅ればせながら[Octopress](http://octopress.org/)を始めてみました。

単にはてな記法を調べるのに疲れたのです。。。  
ほら、最近はほとんどMarkdownでメモ書きますし。

インストールはこんな感じでした。

{% codeblock %}
$ git clone git://github.com/imathis/octopress.git octopress
$ cd octopress
$ bundle install --path vendor
$ rake setup_github_pages # リポジトリのURL聞かれるので答える
$ rake install
{% endcodeblock %}

デフォルトでも文字が大きめでコードハイライトが楽なのがいいですね。  
なるべく楽をしたいですからね。
