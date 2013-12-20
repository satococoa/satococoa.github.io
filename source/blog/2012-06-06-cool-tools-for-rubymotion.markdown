---
layout: post
title: "RubyMotionと使っているツール"
date: 2012-06-06 10:22
tags: RubyMotion
---
最近仕事でRubyMotionを使っています。

まだまだ開発を効率化する方法を模索中なのですが、みなさんのおすすめなども知りたいと思い、まずは今自分が使っていて便利だなーと思うものを共有したいと思います。

より良い方法とか、「これ使うと効率上がるよ！」というのがあったら @satococoa まで教えて下さい。


## vim
エディタは[TextMate 2](http://blog.macromates.com/2011/textmate-2-0-alpha/)や[Sublime Text 2](http://www.sublimetext.com/2)も試しましたが、指がvimを欲してしまうので結局vimに戻ってきてしまいました。コード補完を重視する人には現時点ではSublime Text 2が一番おすすめだと思います。

ちなみに特にsnippetも使わずにtag補完だけでやっています。


## [Ingredients](http://fileability.net/ingredients/)
{% img /images/201206/ingredients.png %}
iOS SDKのリファレンスを見るのに使います。  
Webで検索するよりはるかにサクサクしていて使いやすいです。


## motion-testflight
[testflight](https://testflightapp.com)での配布を簡単にしてくれるgemです。  
設定手順は公式のページに掲載されています。[Submit RubyMotion Apps to TestFlight](http://www.rubymotion.com/developer-center/articles/testflight/)

rakeタスクでビルドを配布可能になります。超便利。

{% codeblock %}
$ rake testflight note="hoge"
{% endcodeblock %}


## ライブラリ等
日々RubyMotion用のgemが充実してきていますね。  
いくつか僕の知っている範囲でご紹介します。

### [BubbleWrap](https://github.com/mattetti/BubbleWrap)
Rubyっぽくもろもろ書けるようにするラッパー集。

### [nitron](https://github.com/mattgreen/nitron)
こっちはUITableViewControllerとCoreDateに特化したラッパーでしょうか。CoreDataあたりは対応始めたばかりっぽい？

### [rubymotion-sqlite](https://github.com/railsfactory/rubymotion-sqlite)
ActiveRecordっぽいインターフェイスでsqliteを使えるようにしてくれる。

### [sugarcube](https://github.com/fusionbox/sugarcube)
便利なヘルパの寄せ集めみたいな感じ。StringとかSymbolまでもがすごい勢いで拡張されます。Rubyっぽい。


---

まだObjective-Cに移行する可能性もあるのでiOS SDKに親しみたいという目的から僕はそれらを使っていません。ヘルパが必要なときは自分で定義して`app/helpers/`あたりに入れています。

とはいえ、TableViewのdelegate地獄はなかなかきついです。

あと、まだWebのAPIとあれこれするようなアプリを作っていないのですが、railsで作ったAPIとうまいことやる方法もこれから確立していきたいと思います。調べてみたいと思っているのは以下。

- [RestKit](https://github.com/RestKit/RestKit)
- [NSRails](https://github.com/dingbat/nsrails) (+ [AFNetworking](https://github.com/AFNetworking/AFNetworking)?)
- [rubymotion-sqlite](https://github.com/railsfactory/rubymotion-sqlite) (+ [AFNetworking](https://github.com/AFNetworking/AFNetworking)?)

試した方はぜひブログ書いておすすめを教えてください。


## 命名規約
Google Groupの方にも最近投稿がありましたが、基本的にはObjective-C由来のものはcamelCase、その他はRubyのよくある命名規約に則ろうと思っています。  
つまり、変数やメソッド名はsnake\_caseで定数は全部大文字で、などですね。

僕が今書いているアプリではそのへんが混ざってしまっています。どけんかせんといかん。


## テストについて
どう書いていいのやら、というかそもそもiOS SDK自体を全然知らないためにどこをどうテストしていいのかわからず、結局まだ全然書いていません。

気になっているのは [Frank](https://github.com/moredip/Frank) です。cucumber風にiOSのテストがかけるらしいので使ってみたい。もうちょっとiOS SDKに親しんで、テストしたい項目がわかってきたら。


## 【宣伝】RubyMotion勉強会始めます
そろそろ色々試している皆さんはノウハウが溜まってきた頃だと思います。こういうノウハウを貯めておく場を作りたいと思いますし、これを共有する場がそろそろ欲しいと思っています。

とりあえず言い出しっぺなのでGoogleグループで作ってみました。 [RubyMotion勉強会 in Japan](https://groups.google.com/d/forum/rubymotionjp)

第1回目の勉強会を東京にて6月下旬〜7月上旬くらいにやりたいと思っています。  
内容とかもこれから考えていきたいと思いますので、興味のある方はぜひお気軽に加わってください。

よろしくお願いします！


## 追記
リファレンスを検索するアプリとして [Dash](http://itunes.apple.com/jp/app/dash/id458034879) というアプリを@watson1978さんに教えて頂きました。

{% img /images/201206/dash.png %}

こちらも今試してみた感じだと、さくさくマニュアル引けますね！本来はスニペット管理アプリみたい？

明日一日使ってみようと思います、ありがとうございます。
