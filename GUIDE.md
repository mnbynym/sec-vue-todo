# GUIDE — 公式ドキュメントへの道しるべ

このチュートリアルで使った機能が、[Vue.js 日本語ドキュメント](https://ja.vuejs.org/guide/introduction.html)のどこに書かれているかをまとめたものです。
手を動かして分からなくなったとき、あるいは一通り終えてから復習するときに使ってください。

## 読む前に

- **ドキュメント側の API 切り替えで「Composition API」を選んでください。**
  多くのページはサンプルコードが Options API と Composition API で切り替わるようになっています。
  このチュートリアルは Composition API(`setup()` と `ref()`)で書いているので、Options API のサンプルを写しても動きません。
- 公式のサンプルの多くは `<script setup>` という、単一ファイルコンポーネント(`.vue`)向けの書き方です。
  このチュートリアルの `setup() { ... return { ... } }` に読み替えてください。違いは主に
  「`<script setup>` では `return` を書かなくてよい」という点だけです。

まず全体像をつかみたいときはこの 2 つです。

- [はじめに](https://ja.vuejs.org/guide/introduction.html) — Vue が何をするものか
- [クイックスタート](https://ja.vuejs.org/guide/quick-start.html) — 動かし方の全体像

---

# 基本版

## step 1: 画面の見た目を HTML で作る

Vue の機能はまだ使いません。先に全体像だけ眺めておくとよいです。

- [はじめに](https://ja.vuejs.org/guide/introduction.html)
- [クイックスタート](https://ja.vuejs.org/guide/quick-start.html)

## step 2: Vue を読み込んでマウントする

- [クイックスタート › CDN の Vue を使用する](https://ja.vuejs.org/guide/quick-start.html#using-vue-from-cdn) — ビルドなしで使う構成
- [クイックスタート › インポートマップの有効化](https://ja.vuejs.org/guide/quick-start.html#enabling-import-maps) — `import { createApp } from 'vue'` と書けるようにする仕組み
- [アプリケーションの作成](https://ja.vuejs.org/guide/essentials/application.html) — `createApp()` とは何か
- [アプリケーションの作成 › アプリのマウント](https://ja.vuejs.org/guide/essentials/application.html#mounting-the-app) — `.mount('#app')`
- [アプリケーションの作成 › DOM 内のルートコンポーネントテンプレート](https://ja.vuejs.org/guide/essentials/application.html#in-dom-root-component-template) — HTML に直接テンプレートを書く形（このチュートリアルの書き方）

## step 3: 状態(データ)を用意する

- [リアクティビティーの基礎 › `ref()`](https://ja.vuejs.org/guide/essentials/reactivity-fundamentals.html#ref) — `ref()` と `.value`
- [リアクティビティーの基礎 › ref を使う理由](https://ja.vuejs.org/guide/essentials/reactivity-fundamentals.html#why-refs) — なぜただの変数ではいけないのか
- [リアクティビティーの基礎 › ディープなリアクティビティー](https://ja.vuejs.org/guide/essentials/reactivity-fundamentals.html#deep-reactivity) — 配列やオブジェクトの中身の変更も検知される
- [テンプレート構文 › テキスト展開](https://ja.vuejs.org/guide/essentials/template-syntax.html#text-interpolation) — `{{ }}`

## step 4: TODO を追加する

- [フォーム入力バインディング › 基本的な使い方](https://ja.vuejs.org/guide/essentials/forms.html#basic-usage) — `v-model`
- [フォーム入力バインディング › テキスト](https://ja.vuejs.org/guide/essentials/forms.html#text) — テキスト入力での `v-model`
- [イベントハンドリング › メソッドハンドラー](https://ja.vuejs.org/guide/essentials/event-handling.html#method-handlers) — `@submit="addTodo"`
- [イベントハンドリング › イベント修飾子](https://ja.vuejs.org/guide/essentials/event-handling.html#event-modifiers) — `.prevent`(＝`event.preventDefault()`)
- [リストレンダリング › ミューテーションメソッド](https://ja.vuejs.org/guide/essentials/list.html#mutation-methods) — `push()` で配列に足すと画面も更新される

参考: [イベントハンドリング › キー修飾子](https://ja.vuejs.org/guide/essentials/event-handling.html#key-modifiers) — `@keyup.enter` の書き方。
このチュートリアルでは日本語入力の変換確定で誤って発火するのを避けるため、あえて使わず `<form>` の送信にしています。

## step 5: 一覧を表示する

- [リストレンダリング › `v-for`](https://ja.vuejs.org/guide/essentials/list.html#v-for)
- [リストレンダリング › `key` による状態管理](https://ja.vuejs.org/guide/essentials/list.html#maintaining-state-with-key) — `:key` が必要な理由
- [テンプレート構文 › 属性バインディング](https://ja.vuejs.org/guide/essentials/template-syntax.html#attribute-bindings) — `:id` / `:for`
- [クラスとスタイルのバインディング › オブジェクトへのバインディング](https://ja.vuejs.org/guide/essentials/class-and-style.html#binding-to-objects) — `:class="{ done: todo.completed }"`
- [条件付きレンダリング › `v-if`](https://ja.vuejs.org/guide/essentials/conditional.html#v-if)
- [フォーム入力バインディング › チェックボックス](https://ja.vuejs.org/guide/essentials/forms.html#checkbox) — チェックボックスの `v-model`

参考: [テンプレート構文 › 生の HTML](https://ja.vuejs.org/guide/essentials/template-syntax.html#raw-html) — `v-html` の説明。
`{{ }}` が自動でエスケープしてくれること、`v-html` を安易に使ってはいけないことが書かれています。

## step 6: TODO を削除する

- [イベントハンドリング › インラインハンドラー下でのメソッドの呼び出し](https://ja.vuejs.org/guide/essentials/event-handling.html#calling-methods-in-inline-handlers) — `@click="removeTodo(todo)"` のように引数を渡す
- [リストレンダリング › 配列の置き換え](https://ja.vuejs.org/guide/essentials/list.html#replacing-an-array) — `filter()` で新しい配列に差し替える

## 完成: 残りの件数を表示する

- [算出プロパティ › 基本的な例](https://ja.vuejs.org/guide/essentials/computed.html#basic-example) — `computed()`
- [算出プロパティ › 算出プロパティ vs. メソッド](https://ja.vuejs.org/guide/essentials/computed.html#computed-caching-vs-methods) — キャッシュされるという違い
- [算出プロパティ › getter 関数は副作用のないものでなければならない](https://ja.vuejs.org/guide/essentials/computed.html#getters-should-be-side-effect-free) — 中で値を書き換えない
- [条件付きレンダリング › `v-else`](https://ja.vuejs.org/guide/essentials/conditional.html#v-else)
- [API › 組み込みディレクティブ › `v-cloak`](https://ja.vuejs.org/api/built-in-directives.html#v-cloak) — マウント前のちらつきを隠す

---

# 発展版

## 発展 1: 完了した TODO をまとめて削除する

新しい機能は出てきません。基本版の復習です。

- [算出プロパティ › 基本的な例](https://ja.vuejs.org/guide/essentials/computed.html#basic-example)
- [条件付きレンダリング › `v-if`](https://ja.vuejs.org/guide/essentials/conditional.html#v-if) — 算出プロパティをそのまま条件に使う

## 発展 2: 「すべて / 未完了 / 完了」で絞り込む

- [リストレンダリング › フィルタリング/並べ替えの結果を表示する](https://ja.vuejs.org/guide/essentials/list.html#displaying-filtered-sorted-results) — **算出プロパティで絞り込むのが定石**、と書かれている節
- [条件付きレンダリング › `v-else-if`](https://ja.vuejs.org/guide/essentials/conditional.html#v-else-if)
- [クラスとスタイルのバインディング › HTML クラスのバインディング](https://ja.vuejs.org/guide/essentials/class-and-style.html#binding-html-classes) — 静的な `class` と `:class` が合成されること
- [イベントハンドリング › インラインハンドラー](https://ja.vuejs.org/guide/essentials/event-handling.html#inline-handlers) — `@click="filter = 'all'"` のような短い式

## 発展 3: localStorage に保存する

- [ウォッチャー › 基本の例](https://ja.vuejs.org/guide/essentials/watchers.html#basic-example) — `watch()`
- [ウォッチャー › 監視ソースの種類](https://ja.vuejs.org/guide/essentials/watchers.html#watch-source-types) — 何を監視できるか
- [ウォッチャー › ディープ・ウォッチャー](https://ja.vuejs.org/guide/essentials/watchers.html#deep-watchers) — `{ deep: true }` が必要な理由とコスト
- [リアクティビティーの基礎 › ディープなリアクティビティー](https://ja.vuejs.org/guide/essentials/reactivity-fundamentals.html#deep-reactivity)

参考: [ウォッチャー › コールバックが実行されるタイミング](https://ja.vuejs.org/guide/essentials/watchers.html#callback-flush-timing)

## 発展 4: ダブルクリックで編集する

- [API › `nextTick()`](https://ja.vuejs.org/api/general.html#nexttick)
- [リアクティビティーの基礎 › DOM 更新のタイミング](https://ja.vuejs.org/guide/essentials/reactivity-fundamentals.html#dom-update-timing) — **なぜ `nextTick` を待つ必要があるのか**
- [イベントハンドリング › キー修飾子](https://ja.vuejs.org/guide/essentials/event-handling.html#key-modifiers) / [キーのエイリアス](https://ja.vuejs.org/guide/essentials/event-handling.html#key-aliases) — `@keyup.esc`
- [テンプレート参照](https://ja.vuejs.org/guide/essentials/template-refs.html) — このチュートリアルでは `document.getElementById()` を使いましたが、**Vue 本来のやり方はこちら**です。`v-for` の中で使う方法も書かれています

## 発展 5: 未完了を上、完了を下に並べる

- [リストレンダリング › フィルタリング/並べ替えの結果を表示する](https://ja.vuejs.org/guide/essentials/list.html#displaying-filtered-sorted-results) — **`sort()` は元の配列を変えてしまうので、コピーしてから使う**ことが明記されています
- [リストレンダリング › ミューテーションメソッド](https://ja.vuejs.org/guide/essentials/list.html#mutation-methods) — 元の配列を変えるメソッドの一覧
- [リストレンダリング › 配列の置き換え](https://ja.vuejs.org/guide/essentials/list.html#replacing-an-array)
- [算出プロパティ › 算出した値の変更を避ける](https://ja.vuejs.org/guide/essentials/computed.html#avoid-mutating-computed-value)

## 発展 6: 1 行分をコンポーネントに切り出す

- [コンポーネントの基礎 › コンポーネントの定義](https://ja.vuejs.org/guide/essentials/component-basics.html#defining-a-component)
- [コンポーネントの登録 › ローカル登録](https://ja.vuejs.org/guide/components/registration.html#local-registration) — `components: { TodoItem }`
- [コンポーネントの登録 › コンポーネント名での大文字・小文字の使い方](https://ja.vuejs.org/guide/components/registration.html#component-name-casing)
- [コンポーネントの基礎 › DOM 内テンプレート解析の注意点](https://ja.vuejs.org/guide/essentials/component-basics.html#in-dom-template-parsing-caveats) — **HTML に直接書くときは `<todo-item>` とケバブケースにする必要がある理由**
- [props › props の宣言](https://ja.vuejs.org/guide/components/props.html#props-declaration)
- [props › 一方向のデータフロー](https://ja.vuejs.org/guide/components/props.html#one-way-data-flow) — **子で props を書き換えてはいけない理由**
- [コンポーネントのイベント › イベントの発行と購読](https://ja.vuejs.org/guide/components/events.html#emitting-and-listening-to-events) — `$emit`
- [コンポーネントのイベント › イベントの引数](https://ja.vuejs.org/guide/components/events.html#event-arguments) — 親側の `$event`
- [コンポーネントのイベント › 発行するイベントの宣言](https://ja.vuejs.org/guide/components/events.html#declaring-emitted-events) — `emits` オプション

---

# この先へ

このチュートリアルでは出てきませんでしたが、次に進むなら押さえておきたいものです。

- [単一ファイルコンポーネント](https://ja.vuejs.org/guide/scaling-up/sfc.html) — `.vue` ファイル。テンプレートを文字列で書かなくて済むようになります
- [ツール](https://ja.vuejs.org/guide/scaling-up/tooling.html) — Vite でプロジェクトを作る方法
- [ライフサイクルフック](https://ja.vuejs.org/guide/essentials/lifecycle.html) — `onMounted` など。実務ではほぼ必ず使います
- [コンポーザブル](https://ja.vuejs.org/guide/reusability/composables.html) — `setup()` の中身を再利用できる形に切り出す
- [ルーティング](https://ja.vuejs.org/guide/scaling-up/routing.html) / [状態管理](https://ja.vuejs.org/guide/scaling-up/state-management.html) — 画面が増えてきたら
- [セキュリティー](https://ja.vuejs.org/guide/best-practices/security.html) — `v-html` の危険性など

---

# 逆引き

| 使ったもの | 出てきたところ | 参照 |
| --- | --- | --- |
| `createApp()` / `mount()` | step 2 | [アプリケーションの作成](https://ja.vuejs.org/guide/essentials/application.html) |
| `ref()` / `.value` | step 3 | [リアクティビティーの基礎](https://ja.vuejs.org/guide/essentials/reactivity-fundamentals.html#ref) |
| `{{ }}` | step 3 | [テンプレート構文](https://ja.vuejs.org/guide/essentials/template-syntax.html#text-interpolation) |
| `v-model` | step 4, 5 | [フォーム入力バインディング](https://ja.vuejs.org/guide/essentials/forms.html) |
| `@click` / `@submit` / `.prevent` | step 4, 6 | [イベントハンドリング](https://ja.vuejs.org/guide/essentials/event-handling.html) |
| `v-for` / `:key` | step 5 | [リストレンダリング](https://ja.vuejs.org/guide/essentials/list.html) |
| `:id` / `:for`(属性バインディング) | step 5 | [テンプレート構文](https://ja.vuejs.org/guide/essentials/template-syntax.html#attribute-bindings) |
| `:class` | step 5, 発展 2 | [クラスとスタイルのバインディング](https://ja.vuejs.org/guide/essentials/class-and-style.html) |
| `v-if` / `v-else` / `v-else-if` | step 5, 完成, 発展 2 | [条件付きレンダリング](https://ja.vuejs.org/guide/essentials/conditional.html) |
| `computed()` | 完成, 発展 1, 2, 5 | [算出プロパティ](https://ja.vuejs.org/guide/essentials/computed.html) |
| `v-cloak` | 完成 | [組み込みディレクティブ](https://ja.vuejs.org/api/built-in-directives.html#v-cloak) |
| `watch()` / `deep` | 発展 3 | [ウォッチャー](https://ja.vuejs.org/guide/essentials/watchers.html) |
| `nextTick()` | 発展 4 | [API: 全般](https://ja.vuejs.org/api/general.html#nexttick) |
| `@keyup.esc` | 発展 4 | [イベントハンドリング › キー修飾子](https://ja.vuejs.org/guide/essentials/event-handling.html#key-modifiers) |
| `props` | 発展 6 | [props](https://ja.vuejs.org/guide/components/props.html) |
| `emit` / `emits` | 発展 6 | [コンポーネントのイベント](https://ja.vuejs.org/guide/components/events.html) |
| `components` 登録 | 発展 6 | [コンポーネントの登録](https://ja.vuejs.org/guide/components/registration.html) |
