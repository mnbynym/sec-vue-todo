# 発展 2: 「すべて / 未完了 / 完了」で絞り込む

## ねらい

表示する TODO を絞り込みます。「今どれを選んでいるか」を状態として持ち、
算出プロパティで一覧を作り直す、という形に慣れます。

## 出発点

`index.html` は 発展 1 の完成版です。

## やること

### 1. 絞り込みの状態を持つ

`todos` の下に追加します。

```js
        // 絞り込みの種類: 'all' / 'active' / 'completed'
        const filter = ref('all')
```

### 2. 絞り込んだ一覧を算出プロパティにする

`remainingCount` の上に追加します。

```js
        // 算出プロパティ: 絞り込んだあとの一覧
        const filteredTodos = computed(() => {
          if (filter.value === 'active') {
            return todos.value.filter((t) => !t.completed)
          }
          if (filter.value === 'completed') {
            return todos.value.filter((t) => t.completed)
          }
          return todos.value
        })
```

### 3. `return` に足す

```js
          newTodo,
          todos,
          filter,
          filteredTodos,
          addTodo,
```

### 4. 一覧が `filteredTodos` を使うようにする

```html
              v-for="todo in filteredTodos" :key="todo.id">
```

### 5. 絞り込みボタンを置く

`<hr>` と一覧の間に追加します。

```html
        <!--
          絞り込みボタン:
            - 押された種類を filter に入れる
            - class は静的なもの(btn)と動的なもの(:class)が合成される
        -->
        <div class="btn-group btn-group-sm mb-3" role="group" aria-label="絞り込み">
          <button type="button" class="btn" @click="filter = 'all'"
                  :class="filter === 'all' ? 'btn-primary' : 'btn-outline-primary'">
            すべて
          </button>
          <button type="button" class="btn" @click="filter = 'active'"
                  :class="filter === 'active' ? 'btn-primary' : 'btn-outline-primary'">
            未完了
          </button>
          <button type="button" class="btn" @click="filter = 'completed'"
                  :class="filter === 'completed' ? 'btn-primary' : 'btn-outline-primary'">
            完了
          </button>
        </div>
```

### 6. 「該当なし」のメッセージを足す

「TODO はまだありません」の `<p>` と、件数の `<p>` の間に差し込みます。

```html
        <!-- 絞り込んだ結果が 0 件のとき(TODO 自体はあるとき) -->
        <p v-else-if="filteredTodos.length === 0" class="text-body-secondary">
          この条件に一致する TODO はありません。
        </p>
```

これで `v-if` → `v-else-if` → `v-else` の 3 択になります。

## 知っておくとよいこと

- `@click="filter = 'all'"` のように、テンプレートの中に短い代入を直接書けます。関数にするまでもない処理はこれで十分です
- `class="btn"` と `:class="..."` は**合成されます**。共通のクラスは静的に、切り替わる部分だけ動的に書くときれいです
- `v-if` / `v-else-if` / `v-else` は**隣り合っている必要があります**。間に別の要素を挟むとエラーになります
- 絞り込みを算出プロパティにしておくと、`todos` と `filter` のどちらが変わっても自動で計算し直されます。
  「ボタンを押したら一覧を作り直す」という手続きを自分で書かなくて済むのが、この書き方の利点です
- `filteredTodos` は `'all'` のとき `todos.value` を**そのまま**返しています。コピーではありません。
  これは 発展 5 で効いてくるので覚えておいてください

## 動作確認

- [ ] TODO を 2 件追加し、片方にチェックを入れる
- [ ] 「未完了」を押すと、未完了の 1 件だけが表示される
- [ ] 押されているボタンだけ色が濃くなる(`btn-primary`)
- [ ] 「完了」を押すと、完了の 1 件だけが表示される
- [ ] その状態でチェックを外すと一覧が空になり、「この条件に一致する TODO はありません。」が出る
- [ ] 「すべて」に戻すと 2 件とも表示され、件数の行も戻る

## つまずきやすいところ

- **`v-else-if` のところでエラーが出る** → `v-if` が付いた `<p>` のすぐ隣に置けていません。間にコメント以外の要素を挟まないでください
- **ボタンの見た目が崩れる** → `class="btn"` を消してしまっていませんか。`btn-primary` だけではボタンの形になりません

## 解答

```
diff index.html 解答/index.html
```
