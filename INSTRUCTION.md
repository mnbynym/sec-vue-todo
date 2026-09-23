# Vue.js で作る TODO アプリ ハンズオン

Vue 3 (Composition API) を使って、ブラウザだけで動く TODO アプリを 7 ステップで作ります。
ビルドツールは使いません。1 ファイルの HTML に `<script type="module">` を書いていくだけです。

## 全体の流れ

| ステップ | テーマ | 学ぶこと |
| --- | --- | --- |
| step1 | 画面の見た目を作る | Bootstrap でのレイアウト(Vue はまだ使わない) |
| step2 | Vue を読み込む | importmap / `createApp` / `mount` |
| step3 | 状態を用意する | `ref` / `setup` / テンプレートに公開する仕組み |
| step4 | TODO を追加する | `v-model` / `@submit.prevent` / イベント処理 |
| step5 | 一覧を表示する | `v-for` / `:key` / `:class` / `v-if` |
| step6 | TODO を削除する | イベントハンドラへの引数渡し |
| 完成 | 残り件数を表示する | `computed` / `v-else` |

## フォルダの構成

どのステップも同じ形です。

```
stepN/
  INSTRUCTION.md    ← このステップでやることの説明(まずこれを読む)
  index.html        ← 出発点。これを編集する(中身は step N-1 の完成版)
  解答/index.html   ← このステップの完成版
```

つまり **`stepN/index.html` にコードを足していくと `stepN/解答/index.html` と同じものができあがる** という作りです。
次のステップの `index.html` は、前のステップの解答そのものなので、途中でつまずいても次に進めます。

最終的な成果物は `完成/解答/index.html` です。

## 進め方

1. `stepN/INSTRUCTION.md` を読む
2. `stepN/index.html` を編集する
3. ブラウザで開いて「動作確認」の項目をチェックする
4. 解答と見比べる

```
cd stepN
diff index.html 解答/index.html
```

コメントの文言まで解答と一致させる必要はありません。動きが同じなら OK です。

## 動かし方(重要)

Vue を ES モジュールとして読み込んでいるため、**HTML ファイルをダブルクリックして開いても動きません**。
`file://` では ES モジュールの読み込みがブラウザにブロックされます(step1 だけは Vue を使わないので例外)。

かならずローカルサーバー経由で開いてください。

```
cd sec-vue-todo
python -m http.server 8000
```

そのうえでブラウザから開きます。

- http://localhost:8000/step1/ … 自分で編集するファイル
- http://localhost:8000/step1/解答/ … 解答

VS Code を使っているなら拡張機能「Live Server」でも構いません。

## 必要なもの

- モダンなブラウザ(Chrome / Edge / Firefox など)と、その開発者ツール
- インターネット接続 — Bootstrap と Vue を CDN から読み込みます
- テキストエディタ

## 困ったときは

- **画面が真っ白** → 開発者ツールの Console タブを見る。だいたいここに理由が出ています
- **`{{ }}` がそのまま表示される** → Vue が動いていない。`file://` で開いていないか、Console にエラーが出ていないか確認
- **どこが違うか分からない** → `diff index.html 解答/index.html`

開発版の Vue を読み込んでいるので、書き方が間違っていると Console に日本語ではありませんが親切な警告が出ます。まず警告を読む習慣をつけてください。
