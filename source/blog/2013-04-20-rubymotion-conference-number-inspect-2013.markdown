---
layout: post
title: "RubyMotion Conference #inspect 2013"
date: 2013-04-20 14:00
categories: RubyMotion event
---
3/28, 29 にベルギーのブリュッセルで開催された [RubyMotion Conference #inspect 2013](http://www.rubymotion.com/conference/) に参加しました。

英語力にだいぶ不安がある中、頑張って聴き取ってきた内容をメモしたいと思います。

### 1 日目

#### A Brave New World: Learning iOS for the Ruby Refugee - Nick Quaranto さん

37signals で働く Nick さんがいかにして RubyMotion で Basecamp for iPhone を作って、どういうことを感じたのか、というお話でした。

Nick さんにとっては Objective-C で開発するということは Xcode というツールを強制されることが苦痛であり、自分の好きなツールを使って自由に開発ができる RubyMotion 無くして Basecamp for iPhone は作れなかっただろう、と話していました。  
Interface Builder を見たときに、まるで Visual Basic を思い出すようだったというところで笑いを取っていました。

また、有用な gem や community のリソースの紹介もされました。

##### Community

- [Google Group](https://groups.google.com/forum/?fromgroups#!forum/rubymotion)
- [https://github.com/rubymotion](https://github.com/rubymotion)
- [http://www.rubymotion.com/developer-center/api/](http://www.rubymotion.com/developer-center/api/)
- [https://github.com/HipByte/RubyMotion-Samples](https://github.com/HipByte/RubyMotion-Samples)

##### Gem

- [bubble-wrap](http://bubblewrap.io), ほとんど全てのプロジェクトで使っている Cocoa Touch の wrapper
- [motion-settings-bundle](https://github.com/qrush/motion-settings-bundle), 設定 app 用の bundle を作る gem
- [motion-layout](https://github.com/qrush/motion-layout), iOS 6.0 から使える auto-layout の gem
- [CocoaPods](http://cocoapods.org) (motion-cocoapods)

ちなみに質疑応答で「テストは書いているか？」という質問がされ、Basecamp アプリはテストを書いていないそうです。  
その後、「テストを書いていますか？」 -> 「書いてないです」というやりとりはほぼ全部の発表後に様式美的に繰り返されることになります。 (1日目後半からは「ノベルティの T シャツはあるのか？」という質問も決まり文句として繰り返されて、笑いを誘っていました。)

RubyMotion でテストを書くときに参考にすべき良いアプリとして、[TinyMon](https://github.com/tkadauke/TinyMon) というアプリが挙げられました。


#### Behaviour Driven Motion using Calabash - Karl Krukow さん

[Carabash](http://calaba.sh) というテスティングフレームワークのお話とデモでした。

会場にその場で挙手を求めてアンケートをとったところ、約 30% の人が RubyMotion のプロジェクトではユニットテストを書いていて、Acceptance Test を書いている人はたったの 2 人でした。

Carabash は Cucumber を利用して受け入れテストを記述するフレームワークで、マルチプラットフォームであることも特徴です。

また、client-server の構成を取っていて、リモートでテストを実行することも可能です。実際にリモートの実機で動作している様子も映像で見せてもらいました。

デモで動かしていたテストのリポジトリは以下です。(RubyMotion-Samples にある Beer アプリに対して受け入れテストを実行しました。)
[https://github.com/krukow/motion-calabash-inspect2013](https://github.com/krukow/motion-calabash-inspect2013)


#### Controlling the Real World with RubyMotion - Rich Kilmer さん

bluetooth のお話でした。

bluetooth の出始めの頃のお話から最近の bluetooth4 の仕様の話という歴史をおさらいするところから始まって、Apple 公式のドキュメントには全く記載のない (でもなぜかサンプルコードは配布されている) bluetooth を使って iPhone をサーバとして周囲の機器をスキャンし、その値を受け取ることもできるそうです。

これも実際にデモをされていました。


#### Elevate your Intent - Matt Green さん

ソフトウェアの設計のお話でした。(RubyMotion に特化した訳ではなく、一般的な概念)

責任の所在をドメインに応じて適宜分散し、小さくてシンプルなクラスを定義することで意図を明確に伝えるソースコードが書けるというお話でした。

Dependencies (依存関係) は複雑さを生み、バグを生み出すので、それをドメインに合うように分離することが大事と説いていました。

結局、依存は厄介なものであって、特に MVC の境界 (サービス層にあたるようなもの) はその依存関係が発生してしまうのでしいですが、それをうまく切り分けてシンプルにし、ドメインに基づいた名前を与え、エラー時には派手にエラーを挙げる (fail loudly) というのがうまく設計するポイントだと話していました。

また、それを支援するライブラリとして [Elevate](https://github.com/mattgreen/elevate) という gem を作って公開されました。

スライド: [Elevate Your Intent](https://speakerdeck.com/mattgreen/elevate-your-intent)

#### Accessibility and RubyMotion - Austin Seraphin

アクセシビリティについての話でした。

生まれつきの全盲である Austin さんの生活がいかにして iPhone の登場によって快適になり、さらに RubyMotion のおかげでいかにソフトウェア開発が容易になったか、というお話をまずはされました。

『カメラを使って色を認識し、その色を音声で教えてくれるアプリを使ってみたけど、何にかざしても "Black!" としか認識されず、最初はアプリが壊れているのかと思ったら、実は夜で電気をつけていなかったために "Black" と認識されていたと気づき、電気をつけた』というエピソードが印象的でした。

Xcode の Interface Builder は全く全盲者にとってはアクセシビリティに乏しく、とても辛いものだそうです。しかし、自分の好きなツールを自由に選択でき、コードで開発ができる RubyMotion はそんな方にも可能性を与えるものとして素晴らしいとのことでした。

その話の後は、実際に開発者はどんな所に気をつけて開発すればいいのか、というところを具体的にレクチャーして頂きました。

スライド: [RUBY MOTION & ACCESSIBILITY](http://www.slideshare.net/AdrianoMartino/ruby-motion-andiosaccessibility)


#### Core Data For The Curious Rubyist - Jonathan Penn さん

資料 (PDF): [Core Data For The Curious Rubyist](http://cocoamanifest.net/features/2013-03-core-data-in-motion.pdf)

Core Data についてのお話でした。

「Core Data は SQLite の ORM ではなくオブジェクトグラフである」という話から始まり、Core Data を使う上で理解する必要のある以下の 5 つの概念についての説明がありました。

- Context (NSManagedObjectContext)
- Object (NSManagedObject)
- Model (NSManagedObejctModel)
- StoreCoordinator (NSPersistentStoreCoordinator)
- FetchedResultsController (NSFetchedResultsController)

特に非同期処理周りはややこしそうですね。。。iOS 5 以上からは `parentContext` というプロパティができてやりやすくはなったらしいですが。

Core Data のモデルファイルをコードから生成できる [Motion Migrate](http://fousa.github.io/motion_migrate/) という gem も紹介されました。

あとは RubyMotion 用の Core Data のラッパーとして以下の2つの gem が挙げられました。

- [MotionData](https://github.com/alloy/MotionData)
- [superbox](https://github.com/awdogsgo2heaven/superbox)


#### The Life and Times of an Object - Josh Ballanco さん

gdb を使った RubyMotion のデバッグの仕方の紹介とデモでした。

RubyMotion では以下のコマンドで dbg が起動します。

`$ rake debug=1`

また、gdb 上で bt(backtrace), b(breakpoint), p(print), pro(print-ruby-object), x(x/4w と打っていた) などを使って実際のデバッグ風景を見せてくれました。

僕が一番驚いたのは、`p (char *)class_getName(<address>)` のようにして Objective-C Runtime の関数が呼べることや、`(char *)[[$1 keys] inspect]` のようにして Ruby のメソッドも呼べてしまうことでした。Ruby 処理系が Objective-C Runtime 上で実装されているおかげなんですね。

最後に `MallocStackLoggingNoCompat=YES rake debug=1` でデバッガを起動した後で `malloc_history` を使ってメモリの確保 / 解放の履歴を見る方法の説明がありました。


#### Concurrency in RubyMotion: Use the Multicore Luke! - Mateus Armando さん

GCD, NSOperationQueue を使った非同期処理についてのお話でした。

GCD, NSOperationQueue の使い方についての全般的な説明、GCD / NSOperationQueue の比較もありました。

[Mateus さんのブログ](http://seanlilmateus.github.io)には他にも GCD 関連のわかりやすい記事が掲載されています。

スライド: [CONCURRENCY PATTERNS IN RUBYMOTION](https://speakerdeck.com/seanlilmateus/concurrency-patterns-in-rubymotion)


#### Get More From RubyMotion with RubyMine - Dennis Ushakov さん

RubyMine での開発、デバッグなどのデモでした。

デバッグ用途だけで RubyMine を使うというのも割とありな気がします。


#### Crafting iOS Dev Tools in Redcar - Delisa Mason さん

Ruby で実装された [Redcar](http://redcareditor.com) というエディタの紹介でした。

実際に HTML でプラグインを書くライブコーディングによるデモと、エディタ上でドキュメントを見られたり、デバッグができると言った特徴の説明がありました。

スライド: [Crafting iOS Dev Tools in Redcar, the Ruby Editor](https://speakerdeck.com/kattrali/crafting-ios-dev-tools-in-redcar-the-ruby-editor)


以上で 1 日目が終わり、その日の夜は @watson1978 さんとムール貝食べてきました。

{% img /images/201304/dinner.jpg %}


### 2 日目

#### NSRevolution: How Ruby hackers built the new Objective-C Open Source community - Mattt Thompson さん

Ruby, Objective-C 双方の歴史などを振り返るところから話が始まりました。

Ruby は Smalltalk, perl, eiffel, lisp から強く影響を受けていて、ObjC は Smalltalk, C から影響を (C は影響というよりは、親みたいなものですが) 受けていて、双方ともメッセージパッシングについては Smalltalk の影響が大きいといった感じです。

そして Ruby の影響で Objective-C での開発フローやツールも変わってきているという話に移り、以下のライブラリやツールの紹介がありました。

##### [CocoaPods](http://cocoapods.org)
Bundler for Objective-C

##### [ReactiveCocoa](https://github.com/ReactiveCocoa/ReactiveCocoa)
Functional Reactive Programming Framework

##### [NUI](https://github.com/tombenner/nui)
Stylesheets for iOS

##### [KIF](https://github.com/square/KIF)
Testing framework

##### [Frank](https://github.com/moredip/Frank)
Cucumber for iOS

##### [Cupertino](https://github.com/mattt/cupertino)
CLI for Apple Developer
[Shenzhen](https://github.com/mattt/shenzhen), [Houston](https://github.com/mattt/houston), [Venice](https://github.com/mattt/venice), [Dubai](https://github.com/mattt/dubai)


#### More Than You Need to Know About CocoaPods - Eloy Duran さん

CocoaPods についての説明と、今後の展開についてのお話でした。

例えば [AFNetworking](https://github.com/AFNetworking/AFNetworking) を使おうとした場合、通常は Xcode であちこちをいじらないといけないところ、CocoaPods を使えばとても簡単に使えるというのを実際に動画であらかじめ撮影したデモを見せてもらい、CocoaPods 無しのライブラリのインストールのあまりの煩雑さに会場は大ウケでした。

CocoaPods の .podspec ファイルの運用については、従来は GitHub の Pull Request ベースで人力で運用していたところを、GitHub API と TravisCI API を使って、自動的に Pull Request を出してマージするサーバを作るようにする、というお話がありました。

また、CocoaPods でインストールできるライブラリのドキュメントを集約する [CocoaDocs](http://cocoadocs.org) というサイトができました。(Ruby で言うところの [RubyDoc.info](http://rubydoc.info) ですね。


#### Wrapping iOS in RubyMotion - Clay Allsopp さん

RubyMotion でより Ruby らしくアプリケーションを作るためのラッパーを作るコツのお話でした。

例えば delegate の代わりに callback を使うという例で以下のコードが挙げられていました。

```
# CoreLocation delegate
Location.get do |location|
  p location[:to]
end
```

`Location.get` メソッドはブロックをインスタンス変数に入れて保持し、delegate を `self ` として delegate メソッド内でそのブロックを `call` しているといった具合です。

また、ブロックを書くときも `->(arg1, arg2) {}` の記法を使ったり、成功時と失敗時に別々のブロックを渡すような ObjC のメソッドをラップするときには Ruby では一つのブロックを渡すようにしてブロック中で `if request.success?` みたいにして分岐する方が Ruby らしくなる、という話もありました。

あとは定数や ENUM は Symbol で指定できるようにすると使いやすいね、とか camelCase は snake_case に、演算子のオーバーロード、メタプログラミングなどの話もありましたのでスライドを参照してください。

スライド: [Wrapping iOS with RubyMotion](https://speakerdeck.com/clayallsopp/wrapping-ios-with-rubymotion)


#### Goodbye IB, Hello Teacup - Colin Gray さん

まずは [teacup](https://github.com/rubymotion/teacup) gem の紹介でした。

Layout と Style というオブジェクトを使ってロジックと見た目を切り離して書くことができます。

また、[sweettea](https://github.com/colinta/sweettea) も使うことで、Style をより CSS っぽく書くことも可能になります。

この場で実機で UI を Firebug や Chrome の開発者ツールのようにデバッグができる Kiln (今は名前が変わって [motion-xray](https://github.com/colinta/motion-xray) になっています) という gem の発表がありました。


#### Using BubbleWrap to Quickly Build RubyMotion Apps - Marin Usalj さん

[BubbleWrap](http://bubblewrap.io) の各モジュールの紹介と簡単な使い方のお話でした。


#### Mixing CoffeeScript in RubyMotion apps - Michael Erasmus さん

cross platform の開発のために Web と Native のハイブリッドアプリを作っていて、Web の部分では CoffeeScript を使っているよ、という内容でした。

まだ試しているだけの段階で深いところまではやっていないけれども、ロジックを Web の中の CoffeeScript で書くことで各プラットフォームで再利用できるようにしたい、という狙いだそうです。

Native と CoffeeScript (JavaScript) とのやりとりは iframe を使っているそうです。

#### Building Interactive Data Visualization Charts - Amit Kumar さん

Data visualization ということで、いくつかのグラフを描画するライブラリについての紹介の後、ご自身で作られた gem の紹介でした。

##### UIWebView を使うもの
- [Highcharts JS](http://www.highcharts.com)

##### Native
- Shinobi (有償)
- iOS:Charts (有償)
- CorePlot

このうち、Native かつ Open Source の CorePlot のラッパーが [motion-plot](https://github.com/toamitkumar/motion-plot) です。

スライド: [RubyMotion - Building Interactive Data Visualization Charts](https://speakerdeck.com/toamitkumar/rubymotion-building-interactive-data-visualization-charts)

#### Cocos2D, an Easier Way - Juan Karam さん

ゲーム開発用のフレームワークである Cocos2D と、それとよく組み合わせて使われる物理演算エンジンの Box2D についてのお話でした。

Box2D は C++ で書かれているためにそのままでは RubyMotion から扱うことが出来ず、Objective-C でラッパーを書いてあげる必要があります。

そのあたりに手をつけ、RubyMotion から扱いやすくした gem がこの場で発表された [Joybox](https://github.com/rubymotion/Joybox) です。

ライブコーディングであっという間にゲームを作っていた様子が圧巻でした。

まだまだ未実装だったり、ドキュメントが全然なかったりするので協力者を募集しています。


#### Let's Move with CoreMotion - Akshat Paul さん、Abhishek Nalwaya さん

正直、内容をあんまり覚えていません。

iPhone の Prezi アプリでプレゼンをしていたのですが、途中で Push 通知が来たり、電話がかかってきたりして爆笑した記憶が強く。。。

iOS での加速度センサやジャイロスコープの扱いの話でした。


#### RubyMotion: Past, Present and Future - Laurent Sansonetti さん

Laurent さんのこれまでの半生と、今後の RubyMotion のロードマップについての発表がありました。

##### ロードマップ

- Toolchain improvements
- Code generators
- Profiler (CPU, Memory)
- Code reloading (REPL)
- Static code analysis
- Tutorials
- Enterprise support
- More platforms

この中では、やはり More platforms が気になりますね。(BlackBerry ではないそうです/笑)

あとは Code reloading が便利そう。ちょっとした View の変更とかが再ビルドしなくても確認できるとなるとなかなか嬉しいですね。

次回のカンファレンス、#inspect 2014 は NewYork city か Mexico あたりで開催される予定らしいです。


### まとめ

暗い場所だったのでだいぶブレましたが、アフターパーティで Laurent さんと撮りました。

{% img /images/201304/laurent.jpg %}

英語力の不足をひしひしと感じる中、なんとか発表の内容を理解しようと必死の2日間でした。なかなか参加者の方とコミュニケーションを取ることも大変だったのですが、最終日のアフターパーティではお酒の勢いを借りて色んな人とお話しできてよかったです。

日本でも 5/29 に [RubyMotion Kaigi 2013](http://connpass.com/event/2095/) というイベントを予定していて、Laurent さんにもしゃべってもらう予定です。

15 年以上ぶりの海外で色々戸惑うところも多かったですが、記念すべき初めてのカンファレンスに参加できてとてもよかったです。

あわせて読みたい: [RubyMotion Conference 2013 - Watson's Blog](http://watson1978.github.io/blog/2013/03/31/rubymotion-conference-2013/)






