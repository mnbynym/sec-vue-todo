# step 4: TODO を追加する

## ねらい

入力欄と `newTodo` をつなぎ(`v-model`)、ボタンや Enter キーの操作で `todos` を更新します(イベント処理)。

一覧の描画は step 5 で作るので、このステップでは **画面には何も出ません**。
追加できているかどうかは開発者ツールの Console で確認します。

## 出発点

`index.html` は step 3 の完成版(データを用意しただけの状態)です。

## やること

### 1. 入力フォームを `<form>` にする

`div.input-group` を `<form>` に置き換え、送信されたら `addTodo` を呼ぶようにします。
あわせて入力欄に `v-model`、ボタンに `type="submit"` を付けます。

```html
        <form class="input-group" @submit.prevent="addTodo">
          <input type="text" class="form-control" placeholder="やることを入力してください"
                 v-model="newTodo">
          <button type="submit" class="btn btn-primary">追加</button>
        </form>
```

- `v-model="newTodo"` … 入力欄と `newTodo` の双方向バインディング。打てば `newTodo` が変わり、`newTodo` を変えれば入力欄も変わります
- `@submit` … フォームが送信されたときに実行する処理(`v-on:submit` の省略形)
- `.prevent` … `event.preventDefault()` のこと。フォーム送信によるページ遷移を止めます

### 2. `addTodo()` を実装する

`setup()` の中、`nextId` の下に追加します。

```js
        // TODO を追加する
        function addTodo() {
          const text = newTodo.value.trim()
          if (text === '') {
            return
          }
          todos.value.push({
            id: nextId++,
            text: text,
            completed: false
          })
          newTodo.value = ''

          // 一覧の描画は step 5 で作るので、今はコンソールで中身を確認する
          console.log('現在のTODOリスト:', todos.value)
        }
```

やっていることは 4 つです。

1. 入力された文字の前後の空白を取る
2. 空なら何もしない(空の TODO が増えないように)
3. `todos` に 1 件足す。`id` はカウンターから取り、`completed` は未完了なので `false`
4. 入力欄を空に戻す

### 3. `return` に `addTodo` を足す

```js
        return {
          newTodo,
          todos,
          addTodo
        }
```

関数もテンプレートから使うには `return` に入れる必要があります。

## なぜ `@click` ではなく `<form>` + `@submit.prevent` なのか

「追加」ボタンに `@click="addTodo"` を付けるだけでも動きます。しかしそれだと Enter キーで追加できません。
では入力欄に `@keyup.enter="addTodo"` を足せばよいかというと、**日本語入力で困ったことが起きます**。

「牛乳」と入力して変換を確定するために Enter を押した瞬間、それが「追加の Enter」として扱われ、
まだ入力し終わっていないのに追加されてしまいます。
`event.isComposing`(変換中かどうか)で防ごうとしても、`keyup` の時点では変換がすでに終了していて
`isComposing` は `false` に戻っているため、ガードになりません。

`<form>` の送信なら、この問題が起きません。変換確定の Enter は IME に消費されるので、
ブラウザはフォームを送信しないからです。ボタンのクリックと Enter の両方を 1 か所で扱えるという利点もあります。

## 動作確認

- [ ] 入力欄に文字を打って「追加」ボタンを押すと、Console に `現在のTODOリスト: [{...}]` と出る
- [ ] 追加すると入力欄が空になる
- [ ] 入力欄で Enter を押しても同じように追加される
- [ ] 何も入力せずに追加しても、空白だけを入力して追加しても、何も起きない
- [ ] **日本語入力で「ぎゅうにゅう」と打って変換確定しても追加されない。もう一度 Enter を押すと追加される**
- [ ] 画面のリストは「サンプルの TODO」のまま変わらない(step 5 で対応します)

Console の配列は、三角マークを開くと `id` / `text` / `completed` を確認できます。
このあと画面に表示するのはこのデータです。

## つまずきやすいところ

- **追加するとページが再読み込みされる(入力欄が消えて Console のログも消える)** → `.prevent` を書き忘れています
- **`newTodo.value` ではなく `newTodo` を使ってしまう** → JavaScript の中では `.value` が必要です
- **`addTodo is not defined`** → `return` に `addTodo` を足し忘れています

## 解答

```
diff index.html 解答/index.html
```
