# 発展 1: 完了した TODO をまとめて削除する

## ねらい

「完了した分だけまとめて消す」ボタンを付けます。
新しい書き方は出てきません。本編で学んだ `computed` / `v-if` / `@click` の組み合わせだけで作れます。

## 出発点

`index.html` は `完成/解答/index.html` と同じものです。

## やること

### 1. 完了した件数を数える算出プロパティ

`remainingCount` の下に追加します。

```js
        // 算出プロパティ: 完了した TODO の件数
        const completedCount = computed(() => {
          return todos.value.filter((t) => t.completed).length
        })

        // 完了した TODO をまとめて削除する
        function clearCompleted() {
          todos.value = todos.value.filter((t) => !t.completed)
        }
```

`clearCompleted()` は `removeTodo()` と同じ形です。「消したいもの以外を集めた新しい配列」に差し替えています。

### 2. `return` に足す

```js
          remainingCount,
          completedCount,
          clearCompleted
```

### 3. ボタンを置く

件数を表示している `<p>` の下、`card-body` の中に追加します。

```html
        <!-- 完了した TODO が 1 件以上あるときだけボタンを出す -->
        <button v-if="completedCount > 0" class="btn btn-outline-secondary btn-sm"
                @click="clearCompleted">
          完了した {{ completedCount }} 件を削除
        </button>
```

## 知っておくとよいこと

- 算出プロパティは「表示する値」だけでなく、**`v-if` の条件にもそのまま使えます**。
  `completedCount > 0` のように書けば、完了が 0 件のときはボタンごと消えます
- 押せないボタンを出しておく(`:disabled`)という選び方もあります。どちらが親切かは場面によります
- ボタンの文字にも `{{ completedCount }}` を入れておくと、何件消えるのかが押す前に分かります

## 動作確認

- [ ] TODO を 3 件追加した時点では、ボタンが出ていない
- [ ] 1 件チェックすると「完了した 1 件を削除」というボタンが現れる
- [ ] もう 1 件チェックすると「完了した 2 件を削除」に変わる
- [ ] ボタンを押すと、チェックした分だけが消えて未完了だけが残る
- [ ] 消えたあとはボタンも消える

## 解答

```
diff index.html 解答/index.html
```
