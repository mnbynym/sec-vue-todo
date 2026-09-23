# 発展 5: 未完了を上、完了を下に並べる

## ねらい

表示の順番を並べ替えます。短い課題ですが、**`sort()` は配列そのものを壊す**という
JavaScript の落とし穴を踏まずに書けるかどうかが本題です。

## 出発点

`index.html` は 発展 4 の完成版です。

## やること

### 1. 並べ替えた一覧を算出プロパティにする

`remainingCount` の上に追加します。

```js
        // 算出プロパティ: 表示する順に並べ替えた一覧(未完了が先、完了が後)
        // 算出プロパティは、別の算出プロパティを材料にして組み立てることもできる
        const sortedTodos = computed(() => {
          // sort() は配列そのものを並べ替える。filteredTodos は絞り込みが 'all' のとき
          // todos.value をそのまま返すので、コピーしないと本体の並びを壊してしまう
          return [...filteredTodos.value].sort((a, b) => Number(a.completed) - Number(b.completed))
        })
```

### 2. `return` に足す

```js
          filteredTodos,
          sortedTodos,
          addTodo,
```

### 3. 一覧が `sortedTodos` を使うようにする

```html
              v-for="todo in sortedTodos" :key="todo.id">
```

## 知っておくとよいこと

- **`sort()` は元の配列を並べ替えます**(新しい配列を返す `filter()` や `map()` とは違います)。
  `todos.value.sort(...)` と書くと、保存されるデータの並びまで書き換わってしまいます
- `[...filteredTodos.value]` は配列のコピーを作る書き方です。
  今回は 発展 2 で `filteredTodos` が「絞り込みが `'all'` のときは `todos.value` をそのまま返す」作りになっているので、
  **このコピーは必須**です。試しに `[...]` を外して、並べ替えたあとに再読み込みしてみてください
- `Number(true)` は `1`、`Number(false)` は `0` です。差が負なら前、正なら後ろに並びます
- JavaScript の `sort()` は**安定**なので、完了状態が同じものどうしは元の並びを保ちます
- 算出プロパティは、別の算出プロパティを材料にできます(`todos` → `filteredTodos` → `sortedTodos`)。
  どれか元の値が変わると、必要なところだけ自動で計算し直されます

## 動作確認

- [ ] A、B、C の順に追加すると、A、B、C の順に並ぶ
- [ ] B にチェックを入れると、B が一番下に移動して A、C、B になる
- [ ] B のチェックを外すと、元の A、B、C に戻る
- [ ] A と C にチェックを入れると、未完了の B が先頭に来て B、A、C になる(完了どうしは追加順のまま)
- [ ] 並べ替えたあとに再読み込みしても、保存されている中身の順番は追加順のまま崩れていない

最後の項目は、開発者ツールの Application タブ → Local Storage で `vue-todo-app` の中身を見ると確認できます。

## 解答

```
diff index.html 解答/index.html
```
