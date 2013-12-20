---
layout: post
title: "Qiita 2-day Hackathon"
date: 2013-02-05 02:21
tags: event
---
[Qiita 2-day Hackathon](http://qiitahackathon03.peatix.com) に参加しました。

このイベントは[GitHub API](http://developer.github.com)を使って何らかのアプリケーションを開発しよう！というハッカソンです。

詳しいテーマは当日発表され、「プログラマの問題を解決するサービス」とのことでした。

当日の様子は公式のブログを見ていただくのがいいかなと思います。

[スペシャルゲストも登場して盛り上がったQiita 2-day Hackathon総まとめ！](http://blog.qiita.com/post/42345394076/qiita-2-day-hackathon-report)


## 僕の作ったもの

全くアイディアを持たずに参加したので、一日目は GitHub API を一通り眺めてターミナルから`curl`や [httpclient](https://github.com/nahi/httpclient) で叩いてみただけでほとんど終わっちゃいました。

全体的に GitHub API で何が出来るのかを把握できた後で、残り半日くらいで出来そうなものを考えたところ、自分の GitHub のプロフィールページを簡単に表示できて、 QR コードでシェアできるものならばデモまでもっていけそうかな、ということで作ってみました。

リポジトリはこちらです。（READMEも何もなくてちょっと不親切ですが。）

- [satococoa/GHProfiles](https://github.com/satococoa/GHProfiles)

RubyMotion + Pixate です。せっかくここまで作ったので、Twitter、Facebook、GitHubのプロフィールを表示・共有できるアプリに仕上げて無料で公開したいなぁと思っています。


## まとめ

GitHub API を使うという点、そして2日間という制限の中で各チーム結構アイディアがかぶったりしたのですが、その中でも「おっ」と思うアプリをつくったチームは本当にすごいなぁと思いました。

特に僕と同じようにプロフィールを交換できるアプリを作ったチームのうち、音でデータのやりとりを実装されたチームがいて、その発想は全くなかったので驚きました。

音とか光、使ってみたいなぁ。。。

とても刺激になりました。
