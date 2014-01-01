---
layout: post
title: "RubyMotion で Nimbus CSS を使ってみる"
date: 2012-12-17 20:50
tags: rubymotion
---
この記事は [RubyMotion Advent Calendar 2012](http://www.adventar.org/calendars/18) の17日目の記事です。

## Nimbus?

最近 [新Google Mapsアプリ採用のフレームワーク NimbusKit がいい感じ](http://fladdict.net/blog/2012/12/nimbus-kit.html) で話題になった [NimbusKit](http://nimbuskit.info) というフレームワークに興味を持ってちらっと調べてみたのですが、なんと CSS でアプリの View の定義ができるらしいということがわかりました。

しかも、CSS を変更するとリアルタイムにその変更が反映されるそうじゃないですか！

デモ動画はこちらです。

<iframe width="560" height="315" src="http://www.youtube.com/embed/i_5LbQ8e9BU" frameborder="0" allowfullscreen></iframe>

さっそく、RubyMotion からも使えるか、試してみました。


## インストール

普通に motion-cocoapods でインストールできます。
Rakefile にこんな感じに書いて、いつも通り`$ rake`でOKです。

```ruby
app.pods do
  pod 'Nimbus'
end
```


## コード

Nimbus CSS 関連のコードだけ貼ります。基本的には公式のドキュメントの [Nimbus: Nimbus CSS](http://docs.nimbuskit.info/NimbusCSS.html) にある "Recommended Procedure for Storing Stylesheets" をなぞっているだけです。

app/app_delegate.rb

```ruby
class AppDelegate
  attr_accessor :stylesheet_cache

  def application(application, didFinishLaunchingWithOptions:launchOptions)
    @window = UIWindow.alloc.initWithFrame(App.bounds)

    path_prefix = NIPathForBundleResource(nil, 'css') # resouces/css に CSS を置く
    host = 'http://localhost:8888/' # CSSファイルの変更を監視サーバを指定する。後述。
    @stylesheet_cache = NIStylesheetCache.alloc.initWithPathPrefix(path_prefix)
    @chameleonObserver = NIChameleonObserver.alloc.initWithStylesheetCache(
      @stylesheet_cache, host:host)
    @chameleonObserver.watchSkinChanges

    main_controller = MainController.alloc.initWithNibName(nil, bundle:nil)
    @root_controller = UINavigationController.alloc.initWithRootViewController(main_controller)
    @window.rootViewController = @root_controller

    @window.makeKeyAndVisible

    true
  end
end
```

app/main_controller.rb

```ruby
class MainController < UIViewController
  def dealloc
    # BubbleWrap を使っています。
    App.notification_center.unobserve(@style_observer)
  end

  def initWithNibName(nibNameOrNil, bundle:nibBundleOrNil)
    super
    stylesheet_cache = App.delegate.stylesheet_cache
    stylesheet = stylesheet_cache.stylesheetWithPath('main.css')
    @dom = NIDOM.alloc.initWithStylesheet(stylesheet)
    @style_observer = App.notification_center.observe(NIStylesheetDidChangeNotification, stylesheet) do |notif|
      @dom.refresh
    end
    self.title = 'Nimbus CSS'
    self
  end

  def loadView
    self.view = UIView.new.tap do |v|
      v.backgroundColor = UIColor.underPageBackgroundColor
    end
    bg_view = UIView.alloc.initWithFrame([[10, 10], [300, 300]])
    @dom.registerView(bg_view, withCSSClass:'background')

    label = UILabel.new.tap do |l|
      l.text = 'Hello, Nimbus'
      l.frame = [[20, 20], [200, 100]]
    end
    @dom.registerView(label)

    small_label = UILabel.new.tap do |l|
      l.text = 'small label'
      l.frame = [[20, 130], [200, 50]]
    end
    @dom.registerView(small_label, withCSSClass:'small')

    self.view.addSubview(bg_view)
    self.view.addSubview(label)
    self.view.addSubview(small_label)
  end

  def viewDidUnLoad
    @dom.unregisterAllViews
  end
end
```

resources/css/main.css

```css
.background {
  background-color: red;
}

UILabel {
  font: 24, "Verdana-Bold";
  background-color: clear;
}

.small {
  font-size: 12;
  background-color: rgba(255, 255, 255, 0.7);
}
```

これで実行すると、下図のような結果になるかと思います。

![Nimbus CSS](/images/201212/nimbus.png)

ちなみに、使用できる CSS のプロパティ一覧は [Nimbus: Nimbus CSS](http://docs.nimbuskit.info/NimbusCSS.html) の "Supported CSS Properties" のところにあります。  
本当は view の位置も調整できると嬉しかったのですが、まだ未対応のようです。


## リアルタイムに変更してみる

さて、いよいよリアルタイムに変更してみたいと思います。

前述のように、コード中に CSS ファイルの変更を監視するサーバを指定しました。これは [https://github.com/jverkoey/nimbus](https://github.com/jverkoey/nimbus) から clone してくると src/css/chameleon にあります。

node.js でできたツールですので、node.js の開発環境が整っていない方は適宜`brew install node`などをして入れてください。

```
$ cd /path/to/chameleon
$ npm install
$ node chameleon.js --watch /path/to/resources/css
```

上記のようにすると、サーバーが立ち上がると思います。念のためブラウザで確認してみましょう。

http://localhost:8888/watch にアクセスし、CSS を保存すると変更されたファイルのパスが画面に表示されると思います。

改めて `$ rake` し、`background-color` などを書き換えて遊んでみてください。


## まとめ

このように、RubyMotion と motion-cocoapods を使えば Nimbus のような Objective-C で書かれたフレームワークも簡単に使うことができます。むしろ、Objective-C でやるよりもライブラリのインストールは楽かもしれません。

今回は Nimbus CSS のみを使ってみましたが、Nimbus には他にもダッシュボード風の UI を作るクラスや、iOS 標準アプリの写真アプリみたいな UI を作るクラス、組み込みのブラウザとしてそのまま使えるクラスなども含まれています。

うまく再利用して効率的にアプリ開発をしたいですね！






