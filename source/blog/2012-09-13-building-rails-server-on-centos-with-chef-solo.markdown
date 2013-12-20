---
layout: post
title: "CentOS + chef-solo で rails が動くサーバを作る"
date: 2012-09-13 15:32
categories: rails linux
---
CentOS + chef-solo で rails が動く All in one サーバを準備する

## 目標
VPS を立ちあげてすぐに All-in-one (DB, Web, App 全部入り) で rails アプリケーションが動くシンプルなサーバを立ち上げる。

ローカルの VirtualBox にインストールして検証する。

ちなみに、Linux のセットアップをするときには未だに僕は slicehost のドキュメント [http://articles.slicehost.com](http://articles.slicehost.com) を見ています。
もっとモダンなところがあったら教えて下さい。

chef の概要については [http://wiki.opscode.com/pages/viewpage.action?pageId=24019581](http://wiki.opscode.com/pages/viewpage.action?pageId=24019581) をざっと読んだのみで、あとは使いながら覚えていく戦法。


## 構成
- CentOS 6.3
- Ruby 1.9.3
- node (asset pipeline のため)
- nginx
- passenger
- MySQL

ruby のインストールは、変なことにハマりたくないので rvm も rbenv も使わず、/usr/local に普通にインストールする。


## OS インストール
OS タイプは Red Hat (64bit) を選択。メインメモリは 512MB。
ISO イメージは "CentOS-6.3-x86_64-netinstall.iso" を使用。
ネットワークの設定はアダプタ 1 が NAT、アダプタ 2 をホストオンリーアダプタとし、vboxnet0 をあらかじめ VirtualBox の環境設定から作成しておく。

あとは普通にインストール。ネットインストールなので OS イメージの URL を聞かれる。"http://ftp.iij.ad.jp/pub/linux/centos/6.3/os/x86_64" を指定した。

インストールが終わったら Devices メニューから .iso を取り外して再起動。

システムを最新にしておく。

```
# yum update
```


## 基本設定 (chef が動くところまで)
### ネットワークの設定 (ホストオンリーネットワーク)
eth0 は NAT としてセットアップされているので VM からインターネットはアクセスできる。
ローカルから VM へアクセスできるように、eth1 をセットアップする。

vi /etc/sysconfig/network-scripts/ifcfg-eth1

```
DEVICE="eth1"
BOOTPROTO="static" # 変更
HWADDR="XX:XX:XX:XX:XX:XX"
NM_CONTROLLED="yes"
ONBOOT="yes" # 変更
TYPE="Ethernet"
UUID="xxxxxxxxxx"
IPADDR="192.168.56.10" # 追加
NETMASK="255.255.255.0" # 追加
```
最後の IPADDR, NETMASK は VirtualBox の環境設定から作ったホストオンリーアダプタの設定に合わせる。

ネットワークを再起動して ping が通ることを確認。

```
# service network restart # VM 上で
$ ping 192.168.56.10 # ローカルから
```


### 一般ユーザの作成
deploy というユーザーを作り、そのユーザーでデプロイしたりrailsを動かしたりする。

```
# useradd deploy -G wheel
# passwd deploy
```

sudo できるように。

```
# visudo
```

以下の行のコメントアウトを解除

```
# %wheel  ALL=(ALL) ALL
```

ここから先は deploy ユーザーで作業するので、一度ログアウトして deploy ユーザーでログイン。

**余談**
ここまでやったところで、[Vagrant](http://vagrantup.com) というものの存在を思い出して少し萎えた。

### SSH

ローカルマシンから公開鍵を転送。

```
$ scp ~/.ssh/id_rsa.pub deploy@192.168.56.10:/tmp
bash: scp: コマンドが見つかりません
lost connection
```

あれ、VM 側に scp が入っていない。

VM 側で openssh-clients を入れてから再挑戦して OK。

```
$ sudo yum install openssh-clients
```

改めて共通鍵のコピー。

```
$ mkdir .ssh
$ chmod 700 .ssh
$ mv /tmp/id_rsa.pub .ssh/authorized_keys
$ chmod 600 .ssh/authorized_keys
```

sshd の設定、再起動。

```
$ sudo vi /etc/ssh/sshd_config
# 変更点
Port XXXX # 適当なポートに変更
PubkeyAuthentication yes
PasswordAuthentication no
$ sudo service sshd restart
```

ここから先はローカルからログインして作業する。

```
$ ssh deploy@192.168.56.10 -p XXXX
```

### iptables

```
$ sudo iptables -L # 現状を確認
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         
ACCEPT     all  --  anywhere             anywhere            state RELATED,ESTABLISHED 
ACCEPT     icmp --  anywhere             anywhere            
ACCEPT     all  --  anywhere             anywhere            
ACCEPT     tcp  --  anywhere             anywhere            state NEW tcp dpt:ssh 
REJECT     all  --  anywhere             anywhere            reject-with icmp-host-prohibited 

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         
REJECT     all  --  anywhere             anywhere            reject-with icmp-host-prohibited 

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
$ sudo iptables -F # 一旦すべて削除
$ sudo vi /etc/iptables.up.rules
# http://articles.slicehost.com/assets/2007/9/4/iptables.txt からまるごと。SSH のポートを自分の環境に合わせるのを忘れないように。
*filter


#  Allows all loopback (lo0) traffic and drop all traffic to 127/8 that doesn't use lo0
-A INPUT -i lo -j ACCEPT
-A INPUT ! -i lo -d 127.0.0.0/8 -j REJECT


#  Accepts all established inbound connections
-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT


#  Allows all outbound traffic
#  You can modify this to only allow certain traffic
-A OUTPUT -j ACCEPT


# Allows HTTP and HTTPS connections from anywhere (the normal ports for websites)
-A INPUT -p tcp --dport 80 -j ACCEPT
-A INPUT -p tcp --dport 443 -j ACCEPT


#  Allows SSH connections
#
# THE -dport NUMBER IS THE SAME ONE YOU SET UP IN THE SSHD_CONFIG FILE
#
-A INPUT -p tcp -m state --state NEW --dport XXXX -j ACCEPT


# Allow ping
-A INPUT -p icmp -m icmp --icmp-type 8 -j ACCEPT


# log iptables denied calls
-A INPUT -m limit --limit 5/min -j LOG --log-prefix "iptables denied: " --log-level 7


# Reject all other inbound - default deny unless explicitly allowed policy
-A INPUT -j REJECT
-A FORWARD -j REJECT

COMMIT
$ sudo iptables-restore < /etc/iptables.up.rules
$ sudo service iptables save
```

いったんここで VirtualBox のスナップショットを取っておいた。
**実はssh, iptablesもchefで管理するもの？**

### chef のインストールまで
ruby, chef をインストールするのに必要なところまでインストールする。

```
# ruby インストールに必要なパッケージ
$ sudo yum -y install git gcc gcc-c++ make autoconf openssl-devel zlib-devel readline-devel curl-devel gettext-devel
$ curl -O http://pyyaml.org/download/libyaml/yaml-0.1.4.tar.gz
$ tar xzvf yaml-0.1.4.tar.gz
$ cd yaml-0.1.4
$ ./configure --prefix=/usr/local
$ make
$ sudo make install
$ cd ..

# ruby インストール
$ curl -O http://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.3-p194.tar.gz
$ tar xzvf ruby-1.9.3-p194.tar.gz
$ cd ruby-1.9.3-p194
$ ./configure --prefix=/usr/local --enable-shared --disable-install-doc --with-opt-dir=/usr/local/lib
$ make
$ make test
$ sudo make install
$ cd ..

# /usr/local/bin に ruby が入るので、sudo 時に path が通るようにする
$ sudo visudo
# ":/usr/local/bin"を追加
Defaults    secure_path = /sbin:/bin:/usr/sbin:/usr/bin:/usr/local/bin

# chef インストール
$ sudo gem install chef --no-ri --no-rdoc
```

ここまでで chef のインストール完了。
スナップショットをとっておく。

## chef のリポジトリを作る

```
$ git clone git://github.com/opscode/chef-repo.git
$ cd chef-repo
```

chef-solo の設定を config/solo.rb に作る。

```
file_cache_path "/home/deploy/chef-repo"
cookbook_path "/home/deploy/chef-repo/cookbooks"
role_path "/home/deploy/chef-repo/roles"
log_level :debug
```

## chef の cookbook を作る (もらってくる)
以下のように chef の cookbook が公開されているところが複数ある。

- http://community.opscode.com/cookbooks
- https://github.com/opscode-cookbooks
- https://github.com/cookbooks

もらってきて必要に応じてカスタマイズする。
自分のリポジトリに置いてみました。試行錯誤中。
まだ全然足りていない。

これ、取得元と同期とったりするのは大変ですね。

[satococoa/chef-repo](https://github.com/satococoa/chef-repo)

submoduleにしちゃうと自前でカスタマイズしたときに面倒だし、どうしよう。。。


あとは今回作りたいサーバは All-in-one だが、後で使いやすいように base, web, db に role を分けてみた。


## chef でインストール
サーバに deploy ユーザーでログインした状態で以下を実行。

```
$ git clone git://github.com/satococoa/chef-repo.git
$ sudo chef-solo -c ~/chef-repo/config/solo.rb -j ~/chef-repo/config/all_in_one.json
```

## あとは
ひたすら cookbook を作ったりカスタマイズしたりするのみ。
iptables とかも chef を使ったほうがいいのだろうか。
（インストールするソフトウェアによってはポート開ける必要があるため）
monit とか logrotate とか munin とかもやらねば。

## 参考 URL
- [http://wiki.opscode.com/display/~tily/Home](http://wiki.opscode.com/display/~tily/Home)
- [http://wiki.opscode.com/display/~tily/Chef+Solo](http://wiki.opscode.com/display/~tily/Chef+Solo)
- [chef-soloで作業環境構築の自動化 | ひげろぐ](http://higelog.brassworks.jp/?p=654)


## 追記
このあと2つほどハマッてしまった。

1. VirtualBox の GuestAdditions を入れようとしたら gcc, make, autoconf が必要だった。
1. SELinux 有効の状態で Postfix が起動しなかった。

SELinux の件は潔く無効にした。

```
$ sudo getenforce
Enforcing # => 有効
$ sudo setenforce 0 # => 無効にする
$ sudo getenforce
Permissive
# 続いて、再起動後も無効になるようにする
$ sudo vi /etc/sysconfig/selinux
SELinux=disabled # この一行のみ編集
```

## 追記2
こうして準備したサーバに capistrano で初めてデプロイするときの手順。  
(capistrano-ext を使っている前提)

```
$ cap staging deploy:setup
$ cap staging deploy:check
$ cap staging deploy:update

# サーバ上で
$ rake RAILS_ENV=staging db:setup
$ rake RAILS_ENV=staging db:seed
```

## 追記3

```
$ cap staging deploy:cold
```
これで `rake db:migration` までやってくれるんですね。知らなかった。  
restartじゃなくて、startになるようですし、こちらの方がいいですね。

