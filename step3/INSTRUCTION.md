# step 3: 状態(データ)を用意する

## ねらい

アプリが扱うデータを `ref()` で用意します。
`ref()` で包んだ値は「変わったら画面を描き直す」対象になります。これが Vue の中心的な仕組みです。

## 出発点

`index.html` は step 2 の完成版(Vue をマウントしただけの状態)です。

## やること

`setup()` の中を次のように書き換えます。

```js
      setup() {

        // リアクティブ･プロパティ
        const newTodo = ref('')
        const todos = ref([])

        // 追加した TODO に一意な id を振るためのカウンター
        // (画面に表示しない値なので ref にする必要はない)
        let nextId = 1

        // return したものがテンプレートから使えるようになる
        return {
          newTodo,
          todos
        }
      }
```

用意するのは次の 3 つです。

| 名前 | 中身 | 役割 |
| --- | --- | --- |
| `newTodo` | 文字列 | 入力欄に今入力されている文字 |
| `todos` | 配列 | TODO の一覧。1 件は `{ id, text, completed }` という形にする予定 |
| `nextId` | 数値 | 次に追加する TODO に振る id |

## 動作確認

見た目は step 2 と変わりません。用意できたかどうかは、テンプレートに一時的に次の行を足して確かめます。

```html
        <p>デバッグ: newTodo = 「{{ newTodo }}」 / todos の件数 = {{ todos.length }}</p>
```

- [ ] `デバッグ: newTodo = 「」 / todos の件数 = 0` と表示される
- [ ] Console にエラーが出ていない

確認できたらこの行は消してください。

ついでに実験しておくと理解が進みます。`return` から `todos` を消してみると、`{{ todos.length }}` のところで
「Property "todos" was accessed during render but is not defined」という警告が出ます。
**`setup()` が返したものだけがテンプレートから見える**、という決まりが確認できます。

## 知っておくとよいこと

- `ref()` で包むと、値の読み書きが Vue に監視されるようになります。値が変わると、その値を使っている画面の部分だけが自動で描き直されます
- **JavaScript の中では `.value` を付けて読み書きします**(`newTodo.value = ''`)。一方、テンプレートの中では `{{ newTodo }}` のように `.value` は不要です。ここは最初に必ず混乱するところです
- `nextId` は画面に表示しないので `ref()` にしていません。何でもかんでも `ref()` にする必要はありません
- `todos` の中身がまだ空配列なのは、次の step 4 で追加処理を書くからです

## 解答

```
diff index.html 解答/index.html
```
