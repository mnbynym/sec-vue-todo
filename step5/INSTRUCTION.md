# step 5: 一覧を表示する

## ねらい

`todos` の中身を画面に描きます。このステップがいちばん Vue らしいところです。

- `v-for` … 配列の件数だけ要素を繰り返す
- `:key` … 繰り返した要素を Vue が見分けるための目印
- `v-model` … チェックボックスと `todo.completed` をつなぐ
- `:class` … 条件によってクラスを付け外しする
- `v-if` … 条件によって要素を出す / 出さない

## 出発点

`index.html` は step 4 の完成版(追加はできるが画面には出ない状態)です。

## やること

### 1. `li` を繰り返し描画する

`ul.list-group` の中の `li` に `v-for` と `:key` を足します。

```html
          <li class="list-group-item d-flex justify-content-between align-items-center"
              v-for="todo in todos" :key="todo.id">
```

これで「`todos` の各要素を `todo` という名前で受け取り、その数だけ `li` を作る」という意味になります。

### 2. 行の中身を `todo` の内容にする

```html
            <div class="form-check m-0">
              <input class="form-check-input" type="checkbox"
                     :id="'todo-check-' + todo.id"
                     v-model="todo.completed">
              <label class="form-check-label" :for="'todo-check-' + todo.id"
                     :class="{ done: todo.completed }">
                {{ todo.text }}
              </label>
            </div>
```

変更点は 4 つです。

1. `id="todo-check"` → `:id="'todo-check-' + todo.id"`(**下の説明を読んでください**)
2. `for="todo-check"` → `:for="'todo-check-' + todo.id"`
3. チェックボックスに `v-model="todo.completed"` を足す
4. ラベルに `:class="{ done: todo.completed }"` を足し、中身を `{{ todo.text }}` にする

`:class="{ done: todo.completed }"` は「`todo.completed` が `true` のときだけ `done` クラスを付ける」という意味です。
`done` クラスは step 1 から `<style>` に用意してあった打ち消し線のスタイルです。

### 3. 空のときだけメッセージを出す

```html
        <p v-if="todos.length === 0" class="text-body-secondary">
          TODO はまだありません。上のフォームから追加してください。
        </p>
```

### 4. `console.log` を消す

画面に出るようになったので、`addTodo()` の中の `console.log` はもう不要です。消してください。

## `:id` を一意にする理由

HTML の `id` は 1 つの文書の中で重複してはいけない決まりです。
`v-for` の中で `id="todo-check"` と固定のまま書くと、同じ `id` を持つチェックボックスが件数分並んでしまいます。

そうなると、`for="todo-check"` が指すのは常に「最初に見つかった `id="todo-check"` の要素」になります。
つまり **2 件目のラベルの文字をクリックすると、1 件目のチェックが切り替わる** という不具合になります。
見た目では気づきにくく、原因も分かりにくいバグです。

`:id="'todo-check-' + todo.id"` のように `todo.id` を混ぜて、1 件ごとに違う値になるようにします
(`todo-check-1`, `todo-check-2`, …)。

## `:key` について

`:key` には、その要素を一意に見分けられる値を渡します。ここでは `todo.id` です。

Vue は再描画のとき、`:key` を頼りに「どの要素が同じものか」を判断し、DOM をできるだけ作り直さずに済ませます。
`:key` を省いたり、配列のインデックスを渡したりすると、並びが変わったときに
「チェックを付けた行とは別の行にチェックが移る」といった不具合の原因になります。

## 動作確認

- [ ] TODO を 2 件追加すると 2 行表示される
- [ ] 追加した文字がそのまま行に出ている
- [ ] チェックを入れると、その行に打ち消し線が付いて灰色になる
- [ ] **2 件目のラベルの「文字」をクリックすると、2 件目のチェックだけが切り替わる**(1 件目は変わらない)
- [ ] TODO が 0 件のときだけ「TODO はまだありません。」が出る。1 件でも追加すると消える
- [ ] 開発者ツールの Elements で、チェックボックスの `id` が `todo-check-1`, `todo-check-2` のように違う値になっている

## つまずきやすいところ

- **`v-for` を `ul` に付けてしまう** → `ul` そのものが件数分増えます。繰り返したいのは `li` です
- **`{{ todo.text }}` が空っぽ** → `addTodo()` で `text` というキーで保存しているか確認してください
- **`:class` に `{ done: ... }` ではなく `"done"` と書いてしまう** → 常に打ち消し線が付きます
- **チェックしても打ち消し線が付かない** → `v-model="todo.completed"` と `:class` の条件が同じ `todo.completed` を見ているか確認

## 解答

```
diff index.html 解答/index.html
```
