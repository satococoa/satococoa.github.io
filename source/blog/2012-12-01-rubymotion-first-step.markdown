---
layout: post
title: "RubyMotion のはじめの一歩"
date: 2012-12-01 01:07
comments: true
categories: RubyMotion
---


この記事は [RubyMotion Advent Calendar 2012](http://www.adventar.org/calendars/18) の一日目の記事です。

RubyMotion を買ってみたけれどもまだ具体的なアプリ作りまでは進んでいない方向けに、RubyMotion で手っ取り早くアプリ開発を始める手順をまとめてみます。


## API リファレンスを準備する
RubyMotion での開発は [Cocoa Touch](https://developer.apple.com/jp/technologies/ios/cocoa-touch.html) フレームワークを直接使いますので、Cocoa Touch の API リファレンスを常に引けるようにしておいて下さい。

[RubyMotion API Reference](http://www.rubymotion.com/developer-center/api/index.html) を見るか、iOS アプリの開発を既に経験されている方は Cocoa Touch のドキュメントを見るのが良いと思います。

RubyMotion 勉強会や RubyMotion もくもく会などで聞いてみたところ、[Dash](https://itunes.apple.com/jp/app/dash-docs-snippets/id458034879?mt=12) を使っている人が大多数でした。

Dash用の [RubyMotion Docset](http://rubymotion.com/files/RubyMotion.docset.zip) も最近公開されました。（ファイルへの直リンクになっています。）


## チュートリアル
手前味噌ですが、僕が以前 Rubyist Magazine に書かせていただいた記事がごく簡単なチュートリアルになっています。

[RubyMotion のご紹介](http://jp.rubyist.net/magazine/?0039-IntroductionToRubyMotion)

もう少しアプリケーションとして楽しい物を作りたい方は id:naoya さんの記事を読みながら作ってみるのがおすすめです。

[RubyMotion - naoyaのはてなダイアリー](http://d.hatena.ne.jp/naoya/20120831/1346409758)

あとは [RubyMotionSamples](https://github.com/HipByte/RubyMotionSamples) を動かしてみながら、改造したりして遊ぶのが楽しいと思います。

ここまでで大体の感触を掴んだら、もう少しがっつりしたチュートリアルを一通りやってみましょう。[RubyMotion Tutorial](http://rubymotion-tutorial.com) がおすすめです。日本語への翻訳も途中までやっているのですが、最近全然進めていないです。。。

僕はあまり詳しく見ていないのですが、[RubyMotion Cookbook](http://iconoclastlabs.github.com/rubymotion_cookbook/) も参考になります。
O'Reilly から出ている [iOS 5 Programming Cookbook](http://shop.oreilly.com/product/0636920021728.do) という本に載っているサンプルコードを RubyMotion で実装しています。


## 全体像を理解する
もちろん RubyMotion 以前に iOS アプリの開発をされていた方は Cocoa Touch についてはもう熟知されていると思いますが、僕のように RubyMotion から iOS アプリ開発を始めた人は、チュートリアルを終えただけでは実際にアプリを開発しようと思っても手が止まってしまうかもしれません。

RubyMotion からは離れてしまいますが、Objective-C で書かれた Apple の公式ドキュメントもとても参考になります。
Apple が日本語に翻訳した PDF もあります。
[日本語ドキュメント - Apple Developer](https://developer.apple.com/jp/devcenter/ios/library/japanese.html)

「iOS View Controllerカタログ」、「iOS View Controller プログラミングガイド」、「iOS View プログラミングガイド」あたりはもしまだ読んでいないようでしたら一通り目を通すことをお勧めします。
チュートリアルから先に進むときに必要な知識になります。

コツさえ覚えてしまえば Objective-C で書かれたサンプルコードなどを RubyMotion で実装するのは容易ですので、Objective-C でコードが書かれた書籍も役に立ちます。個人的には [エキスパートObjective-Cプログラミング ― iOS/OS Xのメモリ管理とマルチスレッド](http://tatsu-zine.com/books/objc) がメモリ管理周り(RubyMotion は ARC と同じようなメモリ管理の仕組みが備わっています)とGCDの話をわかりやすく解説していてくれたのでとてもためになりました。


## Ruby らしく書く
Cocoa Touch フレームワークを Ruby らしく利用できるようにラップしたライブラリが色々あります。[RubyMotion wrappers](http://rubymotion-wrappers.com) にまとまっていますが、BubbleWrap は特に使っている人が多いようです。

既存の RubyGems は残念ながら使えないのですが、[motion-cocoapods](https://github.com/HipByte/motion-cocoapods) を使うことで、Objective-C で書かれたライブラリを気軽に利用することができます。

Ruby の特徴であるメタプログラミングを活かしたい方は [RubyMotion Metaprogramming](http://clayallsopp.com/posts/rubymotion-metaprogramming/) のエントリがとても参考になります。


## 困ったときの解決方法
いざアプリを作り始めるとハマることも多々あると思います。

英語ですが、[Stack Overflow](http://stackoverflow.com)、[Googleグループ](https://groups.google.com/forum/?fromgroups#!forum/rubymotion) で検索すると同じ問題でハマっている人もいると思います。

RubyMotion の情報に絞ってしまうと情報量がまだまだ少ないので、積極的に Objective-C のコードが出てきても読み進めていく方がいいです。

それでも解決しない場合などはコミュニティに質問を投げてしまうと良いと思います。
twitter なら #rubymotionjp のハッシュタグをつけてつぶやいてみる、facebook なら [RubyMotion JP](https://www.facebook.com/groups/149315595198329/) に書き込んでみる、などしてみてください。

- [RubyMotionJP](http://rubymotion.jp)  
ドキュメントの翻訳やイベントの情報などが掲載されています
- RubyMotion 勉強会、もくもく会 などに参加する！

## まとめ
iOS アプリの開発、楽しいです。RubyMotion や僕の書いた記事があなたの背中を押しますように。
