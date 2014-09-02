{
  title: "Meteorで簡単なWebサービスつくってみた",
  date:  "2014-09-01",
  description: "Meteor"
}

Meteorで何かちょっとしたWebサービスをつくってみたいと去年から思ってたけど結局つくっておらず、いい加減何かつくりたいと思い、前々からスケジュール調整サービスつくりたいと思っていたのでようやくつくってみました。

[http://boon.meteor.com/](http://boon.meteor.com/)

## Meteorとは？

Meteorについてはここらへんがくわしいです。

- [Meteor](https://www.meteor.com/)
- [体感！JavaScriptで超速アプリケーション開発 －Meteor完全解説](http://gihyo.jp/dev/serial/01/meteor/)
- [JavaScript超初心者向け Meteor メモ (1)](http://qiita.com/tadfmac/items/a63bb85e5cfb12bbbfc8)

## 今回つかったMeteorパッケージ

僕はTypeScriptとSCSSが好きなのでMeteorでもがっつり使いたいと思いこんな感じのパッケージをいれました。

`v0.9.0`対応版です。

```
% meteor add iron:router
% meteor add fourseven:scss
% meteor add orefalo:typescript-compiler
% meteor add handlebars
% meteor add underscore
% meteor add mizzao:bootstrap-3
```

ちなみにMeteorパッケージはここで探せます。

[atmospherejs.com](http://atmospherejs.com/)

### [iron:router](https://github.com/EventedMind/iron-router)

Routerです。Meteorではデファクトスタンダードな存在。

### [fourseven:scss](https://github.com/fourseven/meteor-scss/)

SCSS派には必須です。`*.scss`使えるようになります。

### [orefalo:typescript-compiler](https://github.com/orefalo/meteor-typescript-compiler/)

Meteorで`*.ts`が使えるようになります。ちょっとクセがあり、Template系の書き方がトリッキーになります。

ちなみにdefinitionsファイルを読み込まなくてはいけません。

```
% meteor add orefalo:typescript-libs
```

でインストールできます。更新頻度があまり高くないので個別に`*.d.ts`を持ってくるほうがいいかもしれません。

こんな感じに`*.ts`の中でdefinitionsファイルを読み込みます。TypeScriptユーザーにはお馴染みですね。

```js
///<reference path="../definitions/lib.d.ts"/>
///<reference path="../definitions/meteor.d.ts"/>
///<reference path="../definitions/underscore.d.ts"/>
///<reference path="../definitions/jquery.d.ts"/>
///<reference path="../definitions/ironrouter.d.ts"/>
///<reference path="../definitions/bootstrap.datepicker.d.ts"/>
///<reference path="../definitions/bootstrap.d.ts"/>

///<reference path="../interface/collections.d.ts"/>
```

> JS

```js
Template.hoge.helpers({...});
Template.hoge.events({...});
Template.hoge.ahya = function() {return 'ahya'}
```

> TS

```js
Template['hoge'].helpers({...});
Template['hoge'].events({...});
Template['hoge']['ahya'] = function() {return 'ahya'}
```

となります。

Meteor.Collectionも`interface`を書かなきゃいけないです。

> JS

```js
UsersCollection = new Meteor.Collection("users");
```

> TS

```js
interface IUsersCollection {
    _id?: string;
    name: string;
}

declare var UsersCollection: Meteor.Collection<IUsersCollection>;

UsersCollection = new Meteor.Collection("users");
```

こんな感じです。

### [handlebars](http://handlebarsjs.com/)

言わずとしれた有名なテンプレートエンジンです。Meteorのデフォルトは`[Spacebars](http://docs.meteor.com/#livehtmltemplates)`という`handlebars`にインスパイアされてつくられたものだそうです。僕は普通にhandlebars使いたいのでいれました。

### [underscore](http://underscorejs.org/)

こちらも超絶便利ですね！`underscore`なしではいきていけません。

### [mizzao:bootstrap-3](https://github.com/mizzao/meteor-bootstrap-3/)

デザインは全部bootstrap3に任せちゃいました。bootstrap便利です！

## Meteorで開発してみて思ったこと

### Meteorのいいところ

超簡単！初心者がフルスタックで何かWebアプリつくりたいっていうときにいいかもしれません。<br>
HTML/CSS/JavaScript だけではじめられるのもいいところです。

最近やっと`v0.9.0.1`まであがってMeteoriteも統合されてパッケージ検索サイトの[atmospherejs.com](http://atmospherejs.com/)っていうのもできたし環境がそろってきた感があります。

### Meteorの難しいと思われるところ

とは言えまだプレビュー版なのでアップデートが激しいです。日本人のコミュニティも少ないですし盛り上がってない感がつらいです。
なのでハマったら自分でひたすら追って調べるしかありません。特にMeteorパッケージあたりを追うのはけっこう大変かも。

(iron-routerとかMeteorがv0.9.0にメジャーアップデートする前にv0.9.0対応版出して動かなくなったりしましたしおすし。)

## とりあえず簡単だから楽しい

とは言え簡単で楽しいので開発入門としてWebアプリとかWebサービスつくるには良いと思います。

## 今回つくったソースはこちら

[github.com/funnythingz/meteor-boon](https://github.com/funnythingz/meteor-boon)
