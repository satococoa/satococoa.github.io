---
layout: post
title: "RubyでGoogleReaderAPIにアクセスする"
date: 2012-11-06 22:37
comments: true
categories: ruby
---
## やりたかったこと
あるサイトの過去記事を全部RDF or Atomで取得したかった。


## 手順
[GoogleReaderApi](https://github.com/nudded/GoogleReaderAPI) っていうgemがあったので使ってみた。

実際はBundler使ってプロジェクト以下にインストールしたりしているが省略。

パラメータとかエンドポイントはぐぐったら非公式のドキュメントが出てきたのでそちらを参照。

[http://code.google.com/p/pyrfeed/wiki/GoogleReaderAPI](http://code.google.com/p/pyrfeed/wiki/GoogleReaderAPI)

```
$ gem install GoogleReaderApi
$ pry
> require 'google_reader_api'
> api = GoogleReaderApi::Api.new {:email => 'example@gmail.com', :password => 'password'}
> api.get_link('atom/feed/http://example.com/feed.rdf', {n:10, r:'o', ot:Date.parse('2012-11-01').to_time.to_i})
```

こんな感じでとれたので、あとはSimpleRSSとかのgemを使ってパースするだけだ！と思いきや、1ヶ月以上昔のエントリの情報は取れないらしい。


## cパラメータを使えばいいらしい

```
> c = ''
> rss = SimpleRSS.parse(
    api.get_link(
      'atom/feed/http://kanasoku.blog82.fc2.com/%3Fxml', {c:c}
    ).tap{|xml|
      c = xml.gsub(%r!^.*<gr:continuation>(.+)</gr:continuation>.*$!m, $1)
    })
> # 繰り返し
```

「次を読む」の要領でどんどんさかのぼれる。  
スクレイピングしないで済みそう。