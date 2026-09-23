# step 1: 画面の見た目を HTML で作る

## ねらい

これから Vue で動かしていく「入れもの」を先に用意します。このステップでは **Vue は一切使いません**。
ここで作った HTML のどこに、どんなデータが流れ込むのかを意識しながら組み立ててください。

## 出発点

`index.html` には次のところまで書いてあります。

- `<head>` … Bootstrap の読み込み、`.done` のスタイル
- `<body>` … カードの外枠(`card` / `card-header` / `card-body`)

`card-body` の中にある `(1)`〜`(4)` のコメントの位置に、中身を書いていきます。

## やること

`card-body` の中に、次の 4 つのブロックを作ります。

1. **入力フォーム** … `div.input-group` の中に、テキスト入力(`input.form-control`)と「追加」ボタン(`button.btn.btn-primary`)
2. **TODO の一覧** … `ul.list-group.mb-3` の中に `li` を 1 つ。行の中はチェックボックス＋ラベルと、「削除」ボタン(`button.btn.btn-danger.btn-sm`)
3. **空のときのメッセージ** … `p.text-body-secondary`
4. **件数の表示** … `p` の中に `span.badge.text-bg-secondary` を 2 つ

Bootstrap のクラス名を覚えるのが目的ではないので、下のコードはそのまま写して構いません。

```html
        <!-- 入力フォーム: step 4 で <form> に置き換えて、入力と追加処理をつなぐ -->
        <div class="input-group">
          <input type="text" class="form-control" placeholder="やることを入力してください">
          <button class="btn btn-primary">追加</button>
        </div>

        <hr>

        <!-- リスト表示: step 5 で v-for を使い、この <li> を TODO の数だけ繰り返し描画する -->
        <ul class="list-group mb-3">
          <li class="list-group-item d-flex justify-content-between align-items-center">
            <div class="form-check m-0">
              <input class="form-check-input" type="checkbox" id="todo-check">
              <label class="form-check-label" for="todo-check">
                サンプルの TODO
              </label>
            </div>
            <button class="btn btn-danger btn-sm">削除</button>
          </li>
        </ul>

        <!-- 空のときのメッセージ: step 5 で v-if を付け、TODO が 1 件もないときだけ表示する -->
        <p class="text-body-secondary">
          TODO はまだありません。上のフォームから追加してください。
        </p>

        <!-- 件数の表示: 最後(完成)で算出プロパティの値を埋め込む -->
        <p>
          残り <span class="badge text-bg-secondary">0</span> 件 /
          全 <span class="badge text-bg-secondary">0</span> 件
        </p>
```

コメントに「step 4 で〜」と書いてあるのは、後のステップでここを書き換えるという予告です。

`li` の中身や「0」は、後のステップで Vue のデータに置き換わる仮の値です。

## 動作確認

このステップは Vue を使っていないので、ファイルをダブルクリックして開いても構いません。

- [ ] 青い枠のカードが表示され、入力欄・追加ボタン・1 行のリスト・メッセージ・件数が並んでいる
- [ ] 「追加」「削除」ボタンを押しても何も起こらない(まだ何も書いていないので当然です)
- [ ] チェックボックスは on / off できる。ただし打ち消し線は付かない

## 知っておくとよいこと

- `.done` のスタイルを `<style>` に用意してありますが、まだどこにも使っていません。step 5 で「完了した行にだけこのクラスを付ける」ために使います。
- チェックボックスの `id="todo-check"` とラベルの `for="todo-check"` は対になっていて、**ラベルの文字をクリックしてもチェックが切り替わります**。試してみてください。
  この `id` は、行が 1 つしかない今は問題ありませんが、step 5 で行を繰り返し描画するようになると困ったことになります。そのときにまた触れます。

## 解答

`解答/index.html` にこのステップの完成版があります。

```
diff index.html 解答/index.html
```
