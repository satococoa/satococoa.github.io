---
layout: post
title: "第1回RubyMotion勉強会を開催しました"
date: 2012-08-05 18:07
categories: RubyMotion event
---
報告が遅くなってしまいましたが、7/22(日)に無事に[第1回RubyMotion勉強会](http://connpass.com/event/665/)を開催しましたのでご報告します。

まだRubyMotion自体の発売からあまり時間が経っていないことがあって、みんなで半日かけて勉強する内容がちゃんとあるかなぁ。。。というのが最初の不安でしたが、ふたを開けてみると発表2本、LT3本、オープンセッション2本という充実した内容で開催することができました。


## 発表1 RubyMotionは誰得？ (+ 宣伝)
- 発表者: [@satococoa](https://twitter.com/satococoa)
- 資料: [RubyMotionは誰得？](https://speakerdeck.com/u/satococoa/p/rubymotion-hashui-de-%3F)

<script async class="speakerdeck-embed" data-id="500b40cf4eac5400020565f7" data-ratio="1.299492385786802" src="//speakerdeck.com/assets/embed.js"></script>

実際にRubyMotionを使って仕事をしてみた経験から、現状のRubyMotionはどんな人にお勧めできて、どんな人は待った方がいいのか、というお話をさせていただきました。

このときにお話しした、僕が仕事でRubyMotionで開発した[アプリ](http://itunes.apple.com/jp/app/batteri-by-dapan/id538707566?mt=8)ですが、見事にAppStoreの無料トップを獲得しました！RubyMotionでも問題なく仕事ができることの証明ができたと思います。  

（こっそり宣伝: RubyMotionやRailsでお仕事したいエンジニアさんや、スマートフォンアプリの開発に携わりたいデザイナーさん、ディレクターさんはいらっしゃいませんか？お気軽に@satococoaまで声をおかけください。）


## 発表2 Differences CRuby/MacRuby/RubyMotion
- 発表者: [@watson1978](https://twitter.com/watson1978) さん
- 資料: [Differences CRuby/MacRuby/RubyMotion](https://speakerdeck.com/u/watson/p/rubymotion)

<script async class="speakerdeck-embed" data-id="500a284b4eac540002002cbe" data-ratio="1.3333333333333333" src="//speakerdeck.com/assets/embed.js"></script>

タイトル通り、CRubyとMacRubyの違い、そしてMacRubyとRubyMotionの違いについてお話ししていただきました。

RubyMotionのソースコードのほとんどはMacRubyと同じで、大きな違いは以下の3点だそうです。

- Rubyソースをネイティブの実行ファイルにコンパイル
- 添付ライブラリは廃止 (Kernel#requireメソッドも動かない)
- GCを独自に実装

GC周りは今後RubyMotionもよりよいものに実装が変わるらしいですし、それがMacRubyの方へも実装されるかもしれない、という耳寄りな情報もありました。

この資料は、MacRubyやRubyMotionをやる人にとっては必見だと思います。詳しい内容は資料をご参照 or 直接@watson1978さんまで！


## LT1 RubyMotion 1.15で追加されたtest周りの話
- 発表者: [@pchw](https://twitter.com/pchw) さん
- 資料: [RubyMotion 1.15で追加されたtest周りの話](https://speakerdeck.com/u/pchw/p/rubymotion-1-dot-15dezhui-jia-saretatestzhou-rifalsehua)

<script async class="speakerdeck-embed" data-id="500eced96005c30002059d98" data-ratio="1.7297297297297298" src="//speakerdeck.com/assets/embed.js"></script>

1.15で実装された、UIAutomationを利用したテストをRubyで書けてしまうRubyMotionの新機能を、デモを交えて紹介していただきました。ぐりぐりシミュレータが動く様は、まるでSeleniumで書かれたテストを動かしているようでした。


## LT2 SublimeRubyMotionBuilderの紹介
- 発表者: [@haraken3](https://twitter.com/haraken3) さん
- 資料: [SublimeRubyMotionBuilderの紹介](https://speakerdeck.com/u/haraken3/p/sublimerubymotionbuilderfalseshao-jie)

<script async class="speakerdeck-embed" data-id="500bf388d4056800020008b7" data-ratio="1.3333333333333333" src="//speakerdeck.com/assets/embed.js"></script>

当日会場で使っているエディタについて尋ねたところ、一番多かったのがSublime Text 2でした。 (ちなみに次点はEmacs)  
そのSublime Text 2用のRubyMotionプラグインの作者様に作ろうと思ったきっかけやプラグインの紹介などをしていただきました。

このプラグイン、AppStoreの複数の国で有料アプリ1位を獲得したEverClipの作者さんも使っているそうで、もはや完全に世界的に定番となっています。

> I use Sublime Text 2 with the RubyMotion plugin which is extremely handy.

引用: [RubyMotion Success Story: EverClip](http://blog.rubymotion.com/post/27906866028/rubymotion-success-story-everclip)


## LT3 nitronの紹介
- 発表者: [@mah_lab](https://twitter.com/mah_lab) さん

RubyMotion用のgemである、[nitron](https://github.com/mattgreen/nitron)の紹介をしていただきました。

RubyMotionでCoreDataをActiveRecord風に使えるようになっていたり、とても短い記述でTableViewControllerを使ったviewが書けたりする、「未来を感じる」gemでした。

ただし、未来を感じつつも現実はテストコードがゼロというオチもあり。。。。うーん。


## オープンセッション
当日にみんなの話したい/知りたいところを募って、2セッション行うことにしました。

### 1. [RubyMotion Tutorial](http://rubymotion-tutorial.com) をやってみる
まだRubyMotionを購入されていない方もいたので、RubyMotionを使った開発がどのようなものか、という実例を見るという意味も兼ねて、僕がプロジェクタに自分のスクリーンを映しつつ、ブツブツつぶやきながらやれるところまで実際にコードを書いてみました。

### 2. nitronソースコードリーディング
[@mah_lab](https://twitter.com/mah_lab)さんにリードしていただきながら、nitronのソースコードを読んでみました。

まだまだ発展途上という感じもありましたが、今後こういったライブラリが増えてくると、きっとRubyMotionの優位性が上がってくるのかなぁというのが個人的な感想です。


## 今後について
日本語の情報をそろそろまとめていこう、という話が懇親会で挙がり、今後以下のような活動をしていこうと思います。

### 日本語でRubyMotionについて語ったり、相談できる場を作る
Facebook上に既にグループがありますので、そちらを使わせていただくことにしました。  
参加自由ですので、どうぞお気軽に加わってください。

- [RubyMotion JP(facebook group)](https://www.facebook.com/groups/149315595198329/)

### github上で有益なコンテンツの翻訳等を公開する。
- [RubyMotionJP](http://rubymotion.jp)

Github上にOrganizationを作って活動しています。[https://github.com/RubyMotionJP](https://github.com/RubyMotionJP)  
協力していただける方はぜひGithubアカウントを添えて@satococoaまで！

第2回のRubyMotion勉強会は事例等がたまってきた頃にやろうかなと思っています。

それとは別に、場所さえ確保できれば月1回くらいのペースでもくもく&相談ができるといいなぁと、今のところ考えています。

あと、[Rubyist Magazine - るびま](http://jp.rubyist.net/magazine/)に記事を書く予定です。次号に間に合うように絶賛執筆中ですのでお楽しみに。

最後に、ご参加いただきましたみなさんやスタッフ、発表者としてご協力いただいたみなさん、そして会場を提供していただいた[KDDI ウェブコミュニケーションズ](http://www.kddi-webcommunications.co.jp)様、本当にありがとうございました。  
今後ともよろしくお願いいたします。
