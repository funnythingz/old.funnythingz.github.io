{
  title: "Railsのlink_toのブロック構文",
  date:  "2014-07-27",
  description: "Railsでlink_toで生成される要素にDOMを使いたい"
}

こんにちは！funnythingzです。

swiftとかgoとか最近流行ってますね！プログラミング楽しいです(^q^

最近Railsを始めたのですが、Rails凄いです。破壊力半端ないです。

それはさておき、`link_to`ってのがあるのですが、生成される要素の中にDOMを入れたいことがあります。

[link_to](http://railsdoc.com/references/link_to)

普通に書くとこんな感じでViewに埋め込みます。

```erb
<%= link_to "コメント削除", [comment.article, comment], method: :delete, data: { confirm: 'Are you sure?' } %>
```

しかしこれだと`<span class="icon remove"></span>`みたいにDOMをそのまま入れると文字列としてエスケープされて出力されてしまいます。

そこでブロック構文を使います。

```erb
<%= link_to [comment.article, comment], method: :delete, data: { confirm: 'Are you sure?' } do %>
    <span class="icon remove"></span>
<% end %>
```

このようにブロック構文を用いることでDOMを扱えます。

覚えることはいっぱいあるけどいろいろ便利だぞRails！
