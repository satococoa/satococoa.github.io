---
layout: post
title: STCTabView というライブラリを作った
date: 2014-09-07 22:24:28 +0900
tags: iOS, CocoaPods
---

某スマートニュースや某グノシーみたいなタブっぽい UI を実現させるために STCTabView っていうライブラリを作りました。

https://github.com/satococoa/STCTabView

ドキュメントもテストも無いですが、ヘッダ見てもらえば使い方はなんとなくわかる。。。と思います。

詳しくは Example を見ていただければ、と思いますがこんな感じにタブバーを定義できます。


```objc
STCTabItemView *item = [[STCTabItemView alloc] initWithFrame:CGRectZero];
STCTabItemView *item2 = [[STCTabItemView alloc] initWithFrame:CGRectZero];

item.text = @"foo";
item.backgroundColor = [UIColor colorWithRed:26/255.0 green:188/255.0 blue:156/255.0 alpha:1.0];
item.selectedBackgroundColor = [UIColor colorWithRed:22/255.0 green:160/255.0 blue:133/255.0 alpha:1.0];

item2.text = @"bar";
item2.backgroundColor = [UIColor colorWithRed:52/255.0 green:152/255.0 blue:219/255.0 alpha:1.0];
item2.selectedBackgroundColor = [UIColor colorWithRed:41/255.0 green:128/255.0 blue:185/255.0 alpha:1.0];

[self.tabView appendTabItem:item];
[self.tabView appendTabItem:item2];

[self.tabView setSelectedTabIndexChangedHandler:^(TabIndex tabIndex) {
  NSLog(@"%ld 番目のタブが選択されたよ", (long)tabIndex);
}];
```
