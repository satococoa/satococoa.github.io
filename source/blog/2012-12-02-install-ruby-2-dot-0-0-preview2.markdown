---
layout: post
title: "Ruby 2.0.0-preview2 をインストール"
date: 2012-12-02 17:11
comments: true
categories: ruby
---
普通に `rbenv install 2.0.0-preview2` したら openssl を require できなかったので以下の手順を行った。

環境は
- Mac OS X 10.8.2 (10.7.xからのアップグレード)
- brew
- brew でインストールした rbenv, ruby-build を使用
- zsh

```
$ brew install openssl
$ vim .zshrc
# 以下を追記
export CONFIGURE_OPTS='--with-readline-dir=/usr/local/opt/readline --with-openssl-dir=/usr/local/opt/openssl' 
$ rbenv install 2.0.0-preview2
$ rbenv global 2.0.0-preview2
$ ruby -ropen-uri -e 'p open("https://www.google.com/").read'
OpenSSL::SSL::SSLError: SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed
…(snip)
# 証明書が無いのが原因？
$ curl -O http://curl.haxx.se/ca/cacert.pem
# 証明書はどこに置けば良いの？
$ ruby -ropenssl -e 'p OpenSSL::X509::DEFAULT_CERT_FILE'
"/usr/local/etc/openssl/cert.pem"
$ mv cacert.pem /usr/local/etc/openssl/cert.pem
```