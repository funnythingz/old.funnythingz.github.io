{
  title: "GoのフレームワークのMartiniで簡単なDEMOをつくってみた",
  date:  "2014-08-06",
  description: "GolangとMartini楽しいよ！"
}

こんにちは！funnythingzです。夏です暑いです。

とある友人の紹介でGoが熱いという話を聞きましてそんなに興味なかったのですがGoをはじめてみました。

[Revel](http://revel.github.io/)ってのと[Martini](http://martini.codegangsta.io/)が良い感じらしいので両方試してようと思ったのですが、Martiniの方が推されていたのでMartiniを試してみることにしてみました。

MartiniはRubyの[Sinatra](http://www.sinatrarb.com/)みたいな感じらしいですが、[martini-contrib](https://github.com/martini-contrib)で[render](https://github.com/martini-contrib/render)や[binding](https://github.com/martini-contrib/binding)といったcontribが公開されているのでこれらを必要に応じてインストールすることで便利になるようです。

とりあえずGoもWebFrameworkも初心者でよくわかりませんがシンプルなDEMOをつくってみます。

## つくるもの

- とりあえずルーティングさせたい。
- テンプレートエンジンでレンダリングしたい。

### 材料

- [Martini](http://martini.codegangsta.io/)
- [martini-contrib/render](https://github.com/martini-contrib/render)
- [Twitter Bootstrap3](http://getbootstrap.com/)

### ディレクトリ構成

シンプルに`layout`と`templates`というディレクトリ分けをします。

```shell-session
├── app.go ... ルーティングの処理をします
├── layout
│   ├── index.go ... Indexをレンダリングします
│   └── about.go ... Aboutをレンダリングします
└── templates
    ├── layout.tmpl ... 全体レイアウトテンプレートです
    ├── index.tmpl  ... Indexのテンプレートです
    └── about.tmpl  ... Aboutのテンプレートです
```

### セットアップ

`go get`コマンドで`martini`と`martini-contrib/render`をインストールします。

```shell-session
% go get github.com/go-martini/martini
% go get github.com/martini-contrib/render
```

### ルーティング

Martiniの根幹でもあるルーティングの処理を書いています。

- Get `/` ... Index
- Get `/about` ... About
- Get `その他` ... Indexにリダイレクト

こんな感じのルーティングにします。

> app.go

```go
package main

import(
    "github.com/go-martini/martini"
    "github.com/martini-contrib/render"
    "./Layout"
)

func app() {

    m := martini.Classic()

    m.Use(render.Renderer(render.Options{
        Directory: "templates",
        Layout: "layout",
        Extensions: []string{".tmpl"},
        Charset: "utf-8",
    }))

    m.NotFound(func (r render.Render){
        r.Redirect("/")
    })

    m.Get("/", layout.IndexRender)
    m.Get("/about", layout.AboutRender)

    m.Run()
}

func main() {
    app()
}
```

### テンプレートレイアウト

共通部分のテンプレートレイアウトを用意します。`Twitter Bootstrap3`を使います。
Bootstrap超便利！

`{{yield}}`にIndexとAboutテンプレートがレンダリングされたものが入ります。

> templates/layout.tmpl

```html
<!Doctype html><html lang="ja"><head><meta charset="utf-8">
<title>{{.Title}}</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=0, maximum-scale=10">
<link href="//bootswatch.com/readable/bootstrap.min.css" rel="stylesheet">
<script src="//code.jquery.com/jquery-2.1.1.min.js"></script>
<script src="//underscorejs.org/underscore.js"></script>
<script src="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/js/bootstrap.min.js"></script>
</head>
<body>

  <nav class="navbar navbar-default" role="navigation">
    <div class="container">
      <div class="container-fluid">
        <div class="navbar-header">
          <button type="button" class="navbar-toggle" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1">
            <span class="sr-only">Toggle navigation</span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
          </button>
          <a class="navbar-brand" href="/">Martini Demo</a>
        </div>

        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
          <ul class="nav navbar-nav navbar-right">
            <li><a href="/about">About me</a></li>
          </ul>
        </div><!-- /.navbar-collapse -->
      </div><!-- /.container-fluid -->
    </div>
  </nav>

  {{yield}}

  <footer>
    <div class="container">
      <p class="text-center">&copy; hogeyan</p>
    </div>
  </footer>

</body></html>
```

### Indexページのレンダリング処理

レンダリング処理を書きます。`github.com/martini-contrib/render`を使います。

`IndexViewModel`というViewModelをつくってそれをIndexRenderに渡してレンダリングします。

> layout/index.go

```go
package layout

import(
    "github.com/martini-contrib/render"
)

type IndexViewModel struct {
    Title string
    Description string
}

func IndexRender(r render.Render) {

    viewModel := IndexViewModel{
        "Martini Demo",
        "Description",
    }

    r.HTML(200, "index", viewModel)
}
```

> templates/index.tmpl

```html
<div class="jumbotron">
  <div class="container">

    <h1>{{.Title}}</h1>
    <p class="lead">{{.Description}}</p>

  </div>
</div>
```

### Aboutページのレンダリング処理

Indexページと同様に処理します。

> layout/about.go

```go
package layout

import(
    "github.com/martini-contrib/render"
)

type Profile struct {
    Name string
    Skill []string
}

type AboutViewModel struct {
    Title string
    Profile Profile
}

func AboutRender(r render.Render) {

    skill := []string{"TypeScript", "Sass/Compass", "Go"}

    profile := Profile{
        "hogeyan",
        skill,
    }

    viewModel := AboutViewModel{
        "About me",
        profile,
    }

    r.HTML(200, "about", viewModel)
}
```

> templates/about.tmpl

```html
<div class="container">

  <h1>{{.Title}}</h1>

  <dl>
    <dt>Name:</dt>
    <dd>{{.Profile.Name}}</dd>
  </dl>

  <dl>
    <dt>Skill:</dt>
    {{range .Profile.Skill}}
    <dd>{{.}}</dd>
    {{end}}
  </dl>

</div>
```

## 動かしてみる

ファイルが準備できたら動かしてみましょう！

```shell-session
% go run app.go
```

go runしたら`http://localhost:3000/`にアクセスしてみましょう。

無事に動いていれば完成です！

> GitHubにもソースを置いておきました。

[github.com/funnythingz/martini-demo/tree/master/render](https://github.com/funnythingz/martini-demo/tree/master/render)
