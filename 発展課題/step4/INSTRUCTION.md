# 発展 4: ダブルクリックで編集する

## ねらい

TODO の文字をダブルクリックすると入力欄になり、書き換えられるようにします。
「今どの行を編集しているか」という一時的な状態の扱いと、`nextTick` を学びます。

## 出発点

`index.html` は 発展 3 の完成版です。

## やること

### 1. `nextTick` を import する

```js
    import { createApp, ref, computed, watch, nextTick } from 'vue'
```

### 2. 編集中の状態を持つ

`filter` の下に追加します。

```js
        // 編集中の TODO の id(編集していないときは null)と、編集中の文字
        const editingId = ref(null)
        const editingText = ref('')
```

編集中の文字を `todo.text` に直接書かず、別の入れものに入れるのがポイントです。
こうしておくと「Esc で取り消す」ができます。

### 3. 3 つの関数を作る

`removeTodo()` の下に追加します。

```js
        // 編集を始める
        async function startEdit(todo) {
          editingId.value = todo.id
          editingText.value = todo.text
          // 入力欄が画面に出るのは次の描画のあとなので、待ってからフォーカスする
          await nextTick()
          document.getElementById('edit-' + todo.id)?.focus()
        }

        // 編集を確定する
        function commitEdit(todo) {
          // Esc で取り消したあとの blur など、すでに編集が終わっていれば何もしない
          if (editingId.value !== todo.id) {
            return
          }
          const text = editingText.value.trim()
          if (text !== '') {
            todo.text = text
          }
          editingId.value = null
        }

        // 編集を取り消す
        function cancelEdit() {
          editingId.value = null
        }
```

### 4. `return` に足す

```js
          removeTodo,
          editingId,
          editingText,
          startEdit,
          commitEdit,
          cancelEdit,
          remainingCount,
```

### 5. 編集中はラベルを入力欄に差し替える

ラベルの `<label ...>` の直前に `<form>` を足し、`<label>` を `v-else` にします。

```html
              <!-- 編集中の行だけ、ラベルの代わりに入力欄を出す -->
              <form v-if="editingId === todo.id" class="d-inline"
                    @submit.prevent="commitEdit(todo)">
                <input type="text" class="form-control form-control-sm"
                       :id="'edit-' + todo.id"
                       v-model="editingText"
                       @blur="commitEdit(todo)"
                       @keyup.esc="cancelEdit">
              </form>
              <label v-else class="form-check-label" :for="'todo-check-' + todo.id"
                     :class="{ done: todo.completed }"
                     @dblclick="startEdit(todo)">
```

`</label>` から下はそのままです。

## 知っておくとよいこと

- **`nextTick` が要る理由**: `editingId` を変えても、入力欄が DOM に現れるのはその直後ではありません。
  Vue は変更をまとめて、次の描画のタイミングで画面に反映します。
  `await nextTick()` は「画面の更新が終わるまで待つ」という意味で、待たずに `focus()` すると対象がまだ存在せず何も起きません
- **`commitEdit()` の先頭のガード**: Esc を押すと入力欄が消え、そのとき `blur` も発生します。
  ガードが無いと、取り消したはずの内容が `blur` 経由で確定してしまいます
- **Enter の確定に `<form>` を使う理由**: 本編 step 4 と同じです。`@keyup.enter` にすると、
  日本語入力の変換確定の Enter で編集が終わってしまいます
- **`?.`(オプショナルチェーン)**: `document.getElementById(...)` が見つからなかった場合に例外にせず、何もしないための書き方です
- **既知の癖**: ラベルのダブルクリックは「クリック 2 回」でもあるため、チェックが 2 回切り替わります。
  結果は元どおりですが一瞬ちらつきます。気になる場合は、文字部分を `<label>` ではなく `<span>` にする手がありますが、
  そうするとラベルをクリックしてチェックする便利さが失われます

## 動作確認

- [ ] TODO の文字をダブルクリックすると入力欄に変わり、カーソルが入っている
- [ ] 入力欄には今の文字が入っている
- [ ] 書き換えて Enter を押すと、確定して表示が変わる
- [ ] 書き換えて Esc を押すと、元の文字のまま戻る
- [ ] 書き換えて入力欄の外をクリックすると、確定する
- [ ] 全部消して確定しようとすると、元の文字が残る(空の TODO にはならない)
- [ ] 日本語を入力して変換を確定しただけでは、編集が終わらない
- [ ] 書き換えてから再読み込みすると、書き換えた内容が残っている(発展 3 の保存が効いています)

## 解答

```
diff index.html 解答/index.html
```
