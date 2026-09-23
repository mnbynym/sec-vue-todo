# step 2: Vue を読み込んでマウントする

## ねらい

Vue を読み込み、「画面のどこを Vue に管理させるか」を決めます。
見た目は step 1 とまったく同じですが、ここから先は `#app` の中が Vue の担当になります。

## 出発点

`index.html` は step 1 の完成版(静的な HTML)です。

## やること

### 1. Vue を読み込む準備をする(`</head>` の直前)

```html
  <!-- インポートマップ: "vue" という名前で ES モジュールビルドを読み込めるようにする -->
  <script type="importmap">
    {
      "imports": {
        "vue": "https://unpkg.com/vue@3/dist/vue.esm-browser.js"
      }
    }
  </script>
```

インポートマップは「`vue` という名前を、この URL に読み替える」という対応表です。
これがあると、次に書く JavaScript で `from 'vue'` と短く書けます。

### 2. アプリを作ってマウントする(`</body>` の直前)

```html
  <script type="module">

    import { createApp, ref, computed } from 'vue'

    createApp({
      setup() {
        // step 3 でここにリアクティブ･プロパティを用意する
        return {}
      }
    }).mount('#app')

  </script>
```

- `type="module"` … ES モジュールとして実行するという宣言。`import` を使うために必要です
- `createApp({ ... })` … アプリの本体を作る
- `setup()` … このアプリが使うデータや関数を用意する場所。返したものがテンプレート(HTML)から使えるようになります
- `.mount('#app')` … `id="app"` の要素を Vue の管理下に置く

`ref` と `computed` は step 3 以降で使います。今は import だけしておいて構いません。

## 動作確認

**ここからはローカルサーバー経由で開いてください。** ダブルクリックで開くと ES モジュールが読み込めずエラーになります。

```
cd sec-vue-todo
python -m http.server 8000
```

→ http://localhost:8000/step2/

- [ ] 見た目が step 1 と変わっていない
- [ ] 開発者ツールの Console タブにエラーが出ていない
- [ ] 開発者ツールの Elements タブで `<div id="app" ...>` を見ると `data-v-app=""` という属性が増えている(マウント成功の印)

## つまずきやすいところ

- **画面は出るが Console に `Failed to load module script` などのエラー** → `file://` で開いています。ローカルサーバー経由で開き直してください
- **`Cannot find module 'vue'`** → インポートマップの書き忘れ、または `<script type="importmap">` を `<script type="module">` より後ろに書いています。インポートマップは先に読み込まれる必要があります
- **`Failed to mount app: mount target selector "#app" returned null`** → `mount()` に渡すセレクタと HTML の `id` が一致していません

## 解答

```
diff index.html 解答/index.html
```
