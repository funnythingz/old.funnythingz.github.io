{
  title: "CabinJSのcode highlightにmonokaiをつくった",
  date:  "2014-07-09",
  description: "monokai大好きです"
}

# CabinJSのcode highlightにmonokaiをつくった

まさかの連続エントリー。CabinJS楽しくなってきたのでいろいろいじってみる。

とりあえずコード書いてみよう！と思い、highlightを試してみたところ、`solarized-dark` がデフォだった。というか`solarized-dark`しかない。`monokai`大好き派としては`monokai`を使いたい！

ということで`monokai`つくりました。早速適当なコードを書いてみましょう。

> JavaScript

```js
var Greetter = (function () {
    function Greetter() {
        this.hello = "hello world";
    }
    Greetter.prototype.greeting = function () {
        return this.hello;
    };
    return Greetter;
})();

var gree = new Greetter();
alert(gree.greeting()); // "hello world"
```

> TypeScript

```js
class Greetter {
    
    hello: string = "hello world";
    
    greeting(): string {
        return this.hello;
    }
}

var gree: Greetter = new Greetter();
alert(gree.greeting()); // "hello world"
```

良い感じです！monokai最高！

[ソースこんな感じ](https://github.com/funnythingz/CabinJS-monokai/blob/master/_monokai.scss)
