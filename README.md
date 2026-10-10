# 仮想ドラフト2026（GitHub Pages ＋ Firebase版）

アカウントなしで、URLを開くだけで遊べる仮想ドラフトです。

| ファイル | 中身 |
|---|---|
| `index.html` | トップページ（1人用とみんなで用の入口） |
| `solo.html` | 1人で遊ぶ（中日目線）。結果の保存は、そのブラウザに保存されます |
| `multi.html` | みんなで遊ぶ。Firebaseでリアルタイムにつながります |
| `firebase-config.js` | Firebaseの接続設定。**ここだけ自分で書き換えます** |

1人用は、GitHubに置くだけで動きます。みんなで遊ぶ方は、Firebaseの設定が必要です（無料、20〜30分）。

---

## 1. Firebaseの準備（みんなで遊ぶ用）

1. https://console.firebase.google.com/ を開き、Googleアカウントでログインします。
2. 「プロジェクトを作成」を押し、名前（例：`draft2026`）を入れて作ります。Googleアナリティクスはオフで大丈夫です。
3. 左のメニューの「構築」→「**Realtime Database**」→「データベースを作成」を押します。
   - 場所は「シンガポール（asia-southeast1）」を選びます。
   - セキュリティルールは「**テストモードで開始**」を選びます。
4. 作成後、「**ルール**」タブを開き、中身を次のように書き換えて「公開」を押します。

```json
{
  "rules": {
    "rooms": {
      "$room": {
        ".read": true,
        ".write": true,
        ".validate": "$room.length <= 24"
      }
    },
    ".read": false,
    ".write": false
  }
}
```

5. 左上の歯車 →「プロジェクトの設定」→ 下の「マイアプリ」で「**</>（ウェブ）**」のアイコンを押します。
   - アプリの名前を入れて「アプリを登録」を押します（Firebase Hostingのチェックは不要）。
6. 表示される `const firebaseConfig = { ... }` の中身を、`firebase-config.js` の `window.FIREBASE_CONFIG = { ... }` にそのまま貼り付けます。
   - `databaseURL` がない場合は、Realtime Databaseの画面の上部に出ているURL（`https://〜.firebasedatabase.app`）を追加します。

## 2. GitHub Pagesで公開

1. https://github.com/ でアカウントを作ります（無料）。
2. 右上の「＋」→「New repository」。名前（例：`draft2026`）を入れ、「**Public**」を選んで「Create repository」。
3. 作ったリポジトリの画面で「uploading an existing file」を押し、このフォルダの5つのファイルをドラッグ＆ドロップして「Commit changes」。
4. 「Settings」→ 左の「Pages」→「Branch」を `main`、フォルダを `/ (root)` にして「Save」。
5. 1〜2分待つと、`https://（ユーザー名）.github.io/draft2026/` で開けるようになります。

## 3. 遊び方（みんなで）

1. 主催する人が `multi.html` を開き、**合言葉**（例：`dragons2026`）を入れて部屋に入ります。
2. 画面に出る「この部屋のURL」をコピーして、LINEなどで友だちに送ります。
3. 主催する人が「この端末で主催する」を押し、全員が担当球団を選んだら「ドラフト開始」。
4. 1位は全員同時に入札し、競合したら封筒を選んで抽選。2位以降はウェーバー順で、持ち時間は3分です。

- 主催する人がページを閉じると、ドラフトが止まります（1時間以内なら開き直すと続きから）。
- 合言葉を知っていれば誰でも入れるので、ほかの人と重なりにくい合言葉にしてください。

## 4. 選手データを更新したいとき

Claudeに頼んで新しい `solo.html` と `multi.html` を作ってもらい、GitHubのリポジトリで同じ名前のファイルをアップロードし直せば上書きされます。`firebase-config.js` はそのままで大丈夫です。

## 注意

- Firebaseの無料枠（Sparkプラン）で、仲間内で遊ぶ規模なら十分です。
- 上のルールでは、部屋のデータは誰でも読み書きできます。個人情報は入れないでください（ニックネームと指名だけを保存しています）。
