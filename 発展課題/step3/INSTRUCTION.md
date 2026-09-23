# 発展 3: localStorage に保存する

## ねらい

ページを閉じても TODO が残るようにします。
データの変化を監視する `watch` を使います。

## 出発点

`index.html` は 発展 2 の完成版です。

## やること

### 1. `watch` を import し、読み込み処理を用意する

`import` の行を書き換え、`createApp(` の前に関数を足します。

```js
    import { createApp, ref, computed, watch } from 'vue'

    // localStorage に保存するときのキー
    const STORAGE_KEY = 'vue-todo-app'

    // 保存されている TODO を読み込む(何も無い / 壊れているときは空の配列)
    function loadTodos() {
      try {
        const json = localStorage.getItem(STORAGE_KEY)
        return json ? JSON.parse(json) : []
      } catch (e) {
        console.error('保存された TODO を読み込めませんでした', e)
        return []
      }
    }
```

`setup()` の外に置いているのは、アプリの状態とは関係のない「ただの道具」だからです。

### 2. 保存されたものを初期値にする

```js
        const todos = ref(loadTodos())
```

### 3. `nextId` の始まりを直す

```js
        // 追加した TODO に一意な id を振るためのカウンター
        // 保存されていた TODO と id がぶつからないよう、最大値の次から始める
        let nextId = todos.value.reduce((max, t) => Math.max(max, t.id), 0) + 1
```

### 4. 変化を監視して保存する

`return` の直前に追加します。

```js
        // todos が変わるたびに保存する
        // deep: true が無いと、配列の中の completed が変わったことに気づけない
        watch(todos, (value) => {
          localStorage.setItem(STORAGE_KEY, JSON.stringify(value))
        }, { deep: true })
```

## 知っておくとよいこと

- `watch(監視するもの, 変わったときの処理, オプション)` という形です
- **`deep: true` が要る理由**: 追加や削除は配列そのものへの操作なので `deep` 無しでも気づけますが、
  チェックの ON/OFF は「配列の中のオブジェクトの `completed` が変わった」だけなので、
  `deep` を付けないと見逃します。一度 `deep: true` を外して、チェックを入れて再読み込みしてみてください
- **`nextId` を `1` のままにしてはいけません**。保存済みの id と重複し、`:key` が同じ行が生まれて表示がおかしくなります
- `JSON.parse()` は壊れた文字列を渡されると例外を投げます。保存データは利用者がいじれる場所にあるので、`try` / `catch` で守っておきます
- `localStorage` に入れられるのは文字列だけなので、`JSON.stringify()` と `JSON.parse()` で変換しています

## 動作確認

- [ ] TODO を追加してページを再読み込みすると、追加した TODO が残っている
- [ ] チェックを入れて再読み込みすると、チェックの状態も残っている
- [ ] 削除して再読み込みすると、削除された状態が残っている
- [ ] 開発者ツールの Application タブ → Local Storage に `vue-todo-app` というキーで保存されている
- [ ] 再読み込みしたあとに新しく追加した TODO の id が、前の続きの番号になっている(Application タブで確認できます)

データを消したいときは、Console で次を実行します。

```js
localStorage.removeItem('vue-todo-app')
```

## つまずきやすいところ

- **`file://` で開いていると、テンプレートと解答が同じデータを共有します**。
  ブラウザからはどちらも同じ置き場所に見えるためです。おかしくなったら上のコマンドで消してください
- **チェックだけが保存されない** → `{ deep: true }` を書き忘れています
- **再読み込みすると表示が崩れる** → `nextId` が `1` のままで、id が重複しています

## 解答

```
diff index.html 解答/index.html
```
