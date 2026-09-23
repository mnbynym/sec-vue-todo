# 発展 6: 1 行分をコンポーネントに切り出す

## ねらい

TODO 1 行分を `TodoItem` というコンポーネントに切り出します。
**`props`(親 → 子)** と **`emit`(子 → 親)** という、Vue の部品化の基本を学びます。

このステップは書き換える量がいちばん多く、これまでで一番難しい課題です。
手が止まったら解答を見ながら進めて構いません。

## 出発点

`index.html` は 発展 5 の完成版です。

## やること

### 1. `TodoItem` コンポーネントを定義する

`createApp({` の**前**に追加します。

```js
    // 1 行分を受け持つコンポーネント
    const TodoItem = {

      // 親から受け取る値
      props: {
        todo: { type: Object, required: true }
      },

      // 親に向けて出すイベント(書いておくと何を出すのか一目で分かる)
      emits: ['toggle', 'edit', 'remove'],

      setup(props, { emit }) {

        // 「編集中かどうか」はこの行だけの事情なので、コンポーネントの中で持つ
        const editing = ref(false)
        const editingText = ref('')

        async function startEdit() {
          editing.value = true
          editingText.value = props.todo.text
          await nextTick()
          document.getElementById('edit-' + props.todo.id)?.focus()
        }

        function commitEdit() {
          if (!editing.value) {
            return
          }
          editing.value = false
          const text = editingText.value.trim()
          if (text !== '') {
            // props は子から書き換えない。「こう変えてほしい」と親に伝える
            emit('edit', text)
          }
        }

        function cancelEdit() {
          editing.value = false
        }

        return { editing, editingText, startEdit, commitEdit, cancelEdit }
      },

      // 1 ファイルで書くときは、テンプレートを文字列で渡す
      template: `
        <li class="list-group-item d-flex justify-content-between align-items-center">
          <div class="form-check m-0">
            <input class="form-check-input" type="checkbox"
                   :id="'todo-check-' + todo.id"
                   :checked="todo.completed"
                   @change="$emit('toggle')">

            <form v-if="editing" class="d-inline" @submit.prevent="commitEdit">
              <input type="text" class="form-control form-control-sm"
                     :id="'edit-' + todo.id"
                     v-model="editingText"
                     @blur="commitEdit"
                     @keyup.esc="cancelEdit">
            </form>
            <label v-else class="form-check-label" :for="'todo-check-' + todo.id"
                   :class="{ done: todo.completed }"
                   @dblclick="startEdit">
              {{ todo.text }}
            </label>
          </div>
          <button class="btn btn-danger btn-sm" @click="$emit('remove')">削除</button>
        </li>
      `
    }
```

### 2. コンポーネントを登録する

```js
    createApp({

      // 使うコンポーネントを登録する
      components: { TodoItem },

      setup() {
```

### 3. 一覧を `<todo-item>` に置き換える

`<ul>` の中の `<li>` から `</li>` までをまるごと次に差し替えます。

```html
          <!--
            1 行分は TodoItem に任せる。
              :todo  … 子に渡す値(props)
              @xxx   … 子から上がってくるイベント。$event には子が渡した値が入る
          -->
          <todo-item v-for="todo in sortedTodos" :key="todo.id"
                     :todo="todo"
                     @toggle="todo.completed = !todo.completed"
                     @edit="todo.text = $event"
                     @remove="removeTodo(todo)"></todo-item>
```

### 4. 親から編集まわりを取り除く

子に移したので、親の `setup()` からは次を削除します。

- `editingId` と `editingText` の定義
- `startEdit()` / `commitEdit()` / `cancelEdit()`
- `return` の中の `editingId, editingText, startEdit, commitEdit, cancelEdit`

親の `setup()` がすっきりするのを確認してください。

## 知っておくとよいこと

- **`props` は親から子への一方通行です。子から書き換えてはいけません。**
  だからチェックボックスは `v-model="todo.completed"` ではなく、
  `:checked="todo.completed"` で表示し、変化したら `@change="$emit('toggle')"` で親に知らせる形にしています。
  実際に書き換えるのは親の仕事です
- `$emit('edit', text)` の第 2 引数が、親側では `$event` に入ります(`@edit="todo.text = $event"`)
- **状態をどちらが持つかの判断**: 「編集中かどうか」はその行だけの事情なので子が持ちます。
  TODO のデータそのものは全体で 1 つなので親が持ちます。この切り分けが部品化の勘どころです
- **タグ名はケバブケース**で書きます(`<todo-item>`)。HTML は大文字小文字を区別しないため、
  HTML に直接書くテンプレートでは `<TodoItem>` は使えません
- `template` を文字列で書けるのは、読み込んでいる Vue が**コンパイラ入りのビルド**だからです。
  ビルドツールを使う構成では、テンプレートは事前に変換されるのでこの書き方は不要になります
- `emits` の宣言は必須ではありませんが、書いておくと「この部品は何を伝えてくるのか」が一目で分かります

## 動作確認

見た目も動きも 発展 5 とまったく同じになれば成功です。

- [ ] 追加・チェック・削除が今までどおり動く
- [ ] チェックすると打ち消し線が付き、行が下に移動する
- [ ] ダブルクリックで編集でき、Enter / Esc / 入力欄の外クリックが今までどおり動く
- [ ] 絞り込みと「完了した N 件を削除」も動く
- [ ] 再読み込みしても内容が残っている
- [ ] 開発者ツールの Elements を見ると、`<todo-item>` というタグは残っておらず `<li>` になっている

## つまずきやすいところ

- **一覧に何も出ない / 警告が出る** → `components: { TodoItem }` の登録忘れ、またはタグ名を `<TodoItem>` と書いています
- **チェックしても反応しない** → `@change="$emit('toggle')"` と、親側の `@toggle="..."` の名前が一致しているか確認してください
- **編集しても文字が変わらない** → `emit('edit', text)` を出していても、親が `@edit="todo.text = $event"` で受けていないと何も起きません

## この先

1 ファイルにコンポーネントを増やしていくと、テンプレートが文字列のままでは補完も色分けも効かず、すぐにつらくなります。
そこが **Vite + 単一ファイルコンポーネント(`.vue`)** に移る頃合いです。
ここまでに学んだ `ref` / `computed` / `watch` / `props` / `emit` は、そのまま `.vue` でも同じように使えます。

## 解答

```
diff index.html 解答/index.html
```
