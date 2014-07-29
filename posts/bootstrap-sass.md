{
  title: "twitter bootstrap-sassをcompassで使うまでのお話",
  date:  "2014-07-29",
  description: "compassでbootstrapを使いたい件について"
}

CSSの設計とかもうやりたくないって思うことってありますよね。すでに世の中にはtwiiter bootstrapやらYUI pureやら優秀なUI-Kitがたくさんあります。今さらオレオレ設計するなんて嫌になっちゃいます。僕はもうすでに嫌です。

そんな感じで案件にbootstrapを使ってみたいけど導入がよくわからんってことで導入までを書いてみます。

## Gemfileをつくる

Sass/Compass/bootstrap は`gem`でインストールします。その`gem`をまとめてインストールするために`Gemfile`をつくります。

エディタで`./Gemfile`を新規で開きます。

```
% vim Gemfile
```

中身はこんな感じです。

```
source 'https://rubygems.org'

gem 'sass'
gem 'compass'
gem 'bootstrap-sass'
```

## bundlerセットアップ

Gemfileができたら`bundle install`コマンドでインストールします。このコマンドは`gem`をまとめてインストールしてくれます。
`bundle install`を使えるようにするために`bundler`をインストールしましょう。

```
% gem install bundler --no-rdoc --no-ri
```

## `bundle install`で`gem`をインストールする

`bundler`がインストールされたら`bundle install`してみましょう。

```
% bundle install
```

これでまとめてインストールされます。

## Compassプロジェクトをbootstrapが使える状態で初期化する

Compassプロジェクトをbootstrapが使える状態で初期化してみましょう。

```
% compass create . -r bootstrap-sass --using bootstrap --css-dir css --images-dir imgs --javascripts-dir js
```

Compassプロジェクトが初期化されました。こんなディレクトリになってると思います。

```
Gemfile
Gemfile.lock
config.rb
css
js
sass
```

## scssファイルをコンパイルしてみる

Compassコマンドでscssファイルをコンパイルしてみましょう。

```
% compass compile
```

コンパイルが通れば環境の完成です！

## scssを編集してみる

あとはガシガシscssを書いてみましょう。追記だったりオーバーライドであれば`sass/styles.scss`を編集すればおｋです。

```
% vim sass/styles.scss
```

単純なbootstrapのカスタマイズであれば`sass/_bootstrap-variables.scss`を編集しましょう。

```
% vim sass/_bootstrap-variables.scss
```

編集したあとはコンパイルしてみましょう。

```
% compass compile
```

コンパイルが通れば完成です！これでCompassでbootstrapが使えるようになりましたね！

お疲れ様でした！
