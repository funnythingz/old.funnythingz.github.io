---
title: Goでの個人開発における本番環境
date: 2015-04-25 20:40 JST
tags:
  - golang
---

こんにちは、funnythingzです。Webサービスやスマホアプリの開発とかやってます。

Goで開発した[plog](http://plog.link)という匿名ダイアリー的なWebサービスがあるのですが、本番環境をいろいろ変えまくったのでメモがてら残しておきます。

READMORE

## さくらVPS

まず最初のリリース時がこの環境です。

さくらVPSのVPS512プランを契約してGoアプリをFastCGIとしてSupervisorでDaemon化してNginx経由で動かすという構成でした。
GoでつくったWebサービスだとこの構成が一番ポピュラーっぽいです。すべてさくらVPS内でもっていました。

データベースとか含むとこんな感じの構成です。

### OS

- CentOS

### 環境構築

- Chefと手動

### Webサーバー

- Nginx
- FastCGI化したGoアプリ
- Supervisor

### DB

- MySQL
- Redis

### 所感

ベタな構成なのですが、すべてがVPS上においてあってサーバー構築も時間かかるしセキュリティ面も自分で考えなきゃだし結構いろいろ面倒くさい。

サーバー構成も忘れやすいし、もしぶっ壊れた場合は復旧が絶対つらいというのが想像できます。運用コスト高いです。

## DigitalOcean

会社でDockerを覚えたのでDocker使うとモテるんじゃないかという下心からとりあえずDocker化してみたいと思いやってみることにしました。
あと[あきやん](https://twitter.com/akiyan)さんに[DigitalOcean](https://www.digitalocean.com/?refcode=f40307411b2b)が良いというのをきいたので招待してもらって移行してみました。

Dockerを使うにあたっては、[tutum.co](https://www.tutum.co/)ってのがありましてDocker Platformな感じのサービスで、DigitalOceanに良い感じに環境を構築してデプロイまでやってくれるというステキサービスがあるのでtutum.coを使いました。

### OS

- Ubuntu

### 環境構築

- Docker
- tutum.co

### Webサーバー

- GoのGoji(コンテナー)

### DB

- MySQL(コンテナー)
- Redis(コンテナー)

### 所感

tutum.coがDockerHubみたいになっていてDockerイメージをprivateで管理することができます。ただしDockerを使うためのOSがデフォルトがUbuntuという謎構成だったのがつらいのと、とりあえず動食っちゃ動くのですが、MySQLコンテナーはデータの永続化させようとすると色々面倒くさいのでMySQLをコンテナーにするのはよくないなぁと思いました。

すべてをDockerのコンテナーとして動かすというのは成功しましたが実際の本番運用はつらいです。

ちなみにGoアプリをDockerにのっけるやりかたはこちらのスライドをどうぞ

<script async class="speakerdeck-embed" data-id="d598024a1bda473eb8fcc563ee644d65" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

## DigitalOcean + Google Cloud SQL

MySQLはやはり外部にもった方がいろいろ扱いやすいなと思い、AWSのRDSは高いしクラウド破産しかねないので[Google Cloud Platform](https://cloud.google.com/products/?hl=ja)のサービス、[Cloud SQL](https://cloud.google.com/sql/?hl=ja)を使うことにしました。

やっぱりMySQLとかデータ系はそれ専用のサービス使うのが一番良いなって思います。

そして、 Docker on Ubuntuをやめて、基本のDocker on CoreOSにしました。

### OS

- CoreOS

### 環境構築

- Docker

### Webサーバー

- GoのGoji(コンテナー)

### DB

- MySQL(Google Cloud SQL)
- Redis(コンテナー)

### 所感

これでやっと安心して運用できるかなと思いました。スケールアウトはできないですが、そこまでアクティブなサービスではないので妥協しています。

## まとめ

こんな感じでGoで個人開発したものを安く公開できる環境は整ってきていると思うので、Go人口が増えたら良いなぁと思ってます。

ちなみに[こちらからDigitalOceanに登録](https://www.digitalocean.com/?refcode=f40307411b2b)すると$10のクーポンがもらえちゃうよ！

Go、楽しいよ、Go!!
