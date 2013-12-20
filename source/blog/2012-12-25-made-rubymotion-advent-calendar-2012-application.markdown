---
layout: post
title: "RubyMotion Advent Calendar 2012 のアプリをつくりました"
date: 2012-12-25 22:06
tags: rubymotion
---
この記事は [RubyMotion Advent Calendar 2012](http://www.adventar.org/calendars/18) の25日目の記事です。

時間がちょっと足りなくてやっつけ感が半端ないですが、Advent Calendar に登録された記事を見られるアプリを作ってみました。

ちょっと見積もりを誤ってしまい、細かい動作や見た目を調整している間がありませんでした＞＜

（具体的には意外とカレンダーの表示に使っているライブラリの自由度が狭くて難儀してました。）

{% img /images/201212/rubymo.png %}

リポジトリは以下です。

[https://github.com/satococoa/RubyMo](https://github.com/satococoa/RubyMo)

カレンダーの表示は [Kal](https://github.com/klazuka/Kal) を使い、エントリの表示には [NimbusKit](http://nimbuskit.info) を使っています。

簡単なサンプル程度のものではありますが、CocoaPods を用いたライブラリの導入や、BubbleWrap での RSS を取得する処理など、参考にしていただけるところもあると思います。


## RubyMotion Advent Calendar 2012 のまとめ

本当に25日分エントリが集まって本当に嬉しく思っています。特に複数の記事を書いていただいた方にはとても感謝しています。

来年 2013 年は [#inspect 2013 - RubyMotion Conference](http://www.rubymotion.com/conference/) もありますし、まだ構想段階ではありますが日本でもカンファレンス(仮)をやりたいと思っています。

今後ユーザーが増えてますます情報の共有が進んでくると、いよいよ仕事で実践投入する方も増えてくることと思います。

来年もユーザー同士でのコミュニケーションが出来る場を作りつつ、自分でもいいアプリをどんどん作っていきたいと思います。

今年お世話になった RubyMotion 関連の方々、本当にありがとうございました。また来年もよろしくお願いします。
