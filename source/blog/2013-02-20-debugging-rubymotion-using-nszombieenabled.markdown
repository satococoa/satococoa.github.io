---
layout: post
title: "RubyMotion のデバッグで NSZombieEnabled を使う"
date: 2013-02-20 19:17
tags: rubymotion
---
RubyMotion でアプリをつくるとき、デバッグがやはり大変です。

例えば GCD など非同期で実行されるブロック内で参照されるオブジェクトをインスタンス変数に入れていない場合、実際にそのブロックの処理が実行されるときには既にそのオブジェクトが解放されてしまっているというケースがあります。  
これが RubyMotion を使う上での一番厄介なハマりどころといえると思います。

そのケースにハマった場合、何も有用なログを残さずにすとんと落ちてしまうことがありとても萎えます。

例えば以下の例はあまりに単純すぎますが、当然アプリがすとんと落ちます。

```
# app_delegate.rb
class AppDelegate
  def application(application, didFinishLaunchingWithOptions:launchOptions)
    main = MainController.new
    @window = UIWindow.alloc.initWithFrame(UIScreen.mainScreen.bounds)
    @window.rootViewController = main
    @window.makeKeyAndVisible
    true
  end
end

# main_controller.rb
class MainController < UIViewController
  def viewDidLoad
    super
    label = UILabel.new.tap do |l|
      l.frame = [[10, 30], [300, 60]]
      l.text = 'hoge'
    end
    label.release # アプリを落とすために意図的に入れてます。
    view.addSubview(label)
  end
end
```

実行するとこうなります。
```
$ rake
     Build ./build/iPhoneSimulator-6.1-Development
   Compile ./app/main_controller.rb
      Link ./build/iPhoneSimulator-6.1-Development/DebugDemo.app/DebugDemo
    Create ./build/iPhoneSimulator-6.1-Development/DebugDemo.dSYM
  Simulate ./build/iPhoneSimulator-6.1-Development/DebugDemo.app
((null))> *** simulator session ended with error: Error Domain=DTiPhoneSimulatorErrorDomain Code=1 "シミュレートした App は終了しました。" UserInfo=0x10014db60 {NSLocalizedDescription=シミュレートした App は終了しました。, DTiPhoneSimulatorUnderlyingErrorCodeKey=-1}
rake aborted!
Command failed with status (1): [DYLD_FRAMEWORK_PATH="/Applications/Xcode.a...]
/Library/RubyMotion/lib/motion/project.rb:101:in `block in <top (required)>'
Tasks: TOP => default => simulator
(See full trace by running task with --trace)
```


このとき、少なくともどのオブジェクト（どのクラスのインスタンス）にアクセスしようとして落ちたのかがわかるだけでもデバッグの助けになります。

以下のように `NSZombieEnabled=YES` という環境変数をつけるとその情報を出すことが出来ます。

```
$ NSZombieEnabled=YES rake
     Build ./build/iPhoneSimulator-6.1-Development
  Simulate ./build/iPhoneSimulator-6.1-Development/DebugDemo.app
2013-02-20 20:38:53.449 DebugDemo[21494:c07] *** -[UILabel superview]: message sent to deallocated instance 0xf1c9f40
(main)> *** simulator session ended with error: Error Domain=DTiPhoneSimulatorErrorDomain Code=1 "シミュレートした App  は終了しました。" UserInfo=0x102252cc0 {NSLocalizedDescription=シミュレートした App は終了しました。, DTiPhoneSimulatorUnderlyingErrorCodeKey=-1}
rake aborted!
Command failed with status (1): [DYLD_FRAMEWORK_PATH="/Applications/Xcode.a...]
/Library/RubyMotion/lib/motion/project.rb:101:in `block in <top (required)>'
Tasks: TOP => default => simulator
(See full trace by running task with --trace)
```

`2013-02-20 20:38:53.449 DebugDemo[21494:c07] *** -[UILabel superview]: message sent to deallocated instance 0xf1c9f40` って出ていますよね？これで、UILabelクラスのインスタンスが原因であることがわかります。

NSZombieEnabled については [NSZombieEnabled - CocoaDev](http://cocoadev.com/wiki/NSZombie) がわかりやすいです。  
ざっくり説明すると、解放されたオブジェクトのクラスを動的に `_NSZombie` に変更し、そのメモリ領域を解放させないようにしているおかげで上記のような情報をログに出してくれているようです。

あとは [Debugging RubyMotion applications](http://rubymotion.jp/RubyMotionDocumentation/articles/debugging/index.html) のページを参照していただいて、`debug`オプションを使ってステップ実行したりするとより詳しくデバッグすることが出来ます。
