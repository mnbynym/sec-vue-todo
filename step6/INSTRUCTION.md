# step 6: TODO を削除する

## ねらい

イベントハンドラに引数を渡して、「どの TODO を消すのか」を伝えます。

## 出発点

`index.html` は step 5 の完成版(一覧表示までできた状態)です。

## やること

### 1. 削除ボタンにクリックイベントを付ける

`li` の中の削除ボタンを次のようにします。

```html
            <!-- イベントハンドラには引数を渡せる。ここではクリックされた todo 自身を渡す -->
            <button class="btn btn-danger btn-sm" @click="removeTodo(todo)">削除</button>
```

このボタンは `v-for` の内側にあるので、その行の `todo` をそのまま渡せます。

### 2. `removeTodo()` を実装する

`addTodo()` の下に追加します。

```js
        // TODO を削除する(渡された todo 以外を集めた新しい配列に差し替える)
        function removeTodo(todo) {
          todos.value = todos.value.filter((t) => t !== todo)
        }
```

`filter()` は「条件に合う要素だけを集めた**新しい配列**」を返します。
`t !== todo` なので、渡された `todo` 以外が残ります。それを `todos.value` に入れ直すと、画面が描き直されます。

### 3. `return` に `removeTodo` を足す

```js
        return {
          newTodo,
          todos,
          addTodo,
          removeTodo
        }
```

## 知っておくとよいこと

- `@click="removeTodo(todo)"` のように書けるのは、テンプレートの中では「その場で実行する式」を書けるからです。
  `@click="removeTodo"` とだけ書いた場合は、クリックイベントのオブジェクトが引数として渡ってしまい、意図した動きになりません
- `id` を使って書くこともできます。どちらでも構いません

  ```js
  todos.value = todos.value.filter((t) => t.id !== todo.id)
  ```

- `splice()` で元の配列を直接削る方法もありますが、`filter()` で作り直すほうが「どの要素を消すか」を探す処理を書かずに済み、間違いにくいです

## 動作確認

- [ ] TODO を 3 件追加し、真ん中の「削除」を押すと、その行だけが消える
- [ ] 残った行の表示(文字・チェックの状態)が崩れていない
- [ ] チェックを付けた行を削除しても、他の行のチェックが動かない
- [ ] 全部削除すると「TODO はまだありません。」が再び表示される

3 件目にチェックを入れてから 1 件目を削除してみてください。
チェックが別の行に移ってしまわないのは、step 5 で `:key="todo.id"` を付けたおかげです。

## つまずきやすいところ

- **押しても何も起きない** → `return` に `removeTodo` を足し忘れていないか確認
- **全部消えてしまう** → `filter` の条件が逆(`t === todo`)になっています
- **`todos.value.filter(...)` の結果を代入し忘れる** → `filter()` は元の配列を変更しません。戻り値を `todos.value` に代入する必要があります

## 解答

```
diff index.html 解答/index.html
```
