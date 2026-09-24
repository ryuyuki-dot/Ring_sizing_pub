# リングサイズメジャー

スマホ画面で指のサイズを測り、USリングサイズを算出するPWA（Progressive Web App）です。
GitHub Pagesで公開すると、Androidでインストール可能な「アプリ」として使えます。

## 構成

```
index.html      本体（測定画面・アカウント/履歴画面）
manifest.json   アプリ名・アイコン・起動設定
sw.js           オフラインでも起動できるようにするservice worker
icons/          アプリアイコン（192px / 512px / maskable）
.github/workflows/deploy.yml   pushすると自動でGitHub Pagesに公開されるActions設定
```

## 1. GitHubにアップロードする

1. GitHubで新しいリポジトリを作成（例: `ring-sizer`）
2. このフォルダの中身一式をそのリポジトリにpush

```bash
cd ring-sizer-app
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/【あなたのユーザー名】/ring-sizer.git
git push -u origin main
```

## 2. GitHub Pagesを有効化する

1. リポジトリの **Settings → Pages** を開く
2. **Source** で「GitHub Actions」を選択（同梱の `deploy.yml` が自動でビルド・公開します）
3. 数分後、`https://【ユーザー名】.github.io/ring-sizer/` でアプリが公開されます

## 3. Androidにインストールする（PWAとして）

1. Android端末の **Chrome** で上記URLを開く
2. メニュー(⋮)から「**アプリをインストール**」または「**ホーム画面に追加**」をタップ
3. ホーム画面にアイコンが追加され、ブラウザのアドレスバーなしで通常のアプリのように起動します
4. `sw.js` によりオフラインでも起動できます

これだけでも「スマホアプリ」として日常利用するには十分です（履歴データは端末内に保存されます）。

## 4. 本格的なAPK（Android単体アプリ）が欲しい場合

Google PlayやAPKファイルとして配布したい場合は、公開したPWAのURLをもとに **Trusted Web Activity（TWA）** としてAndroidアプリ化できます。コードを書く必要はありません。

### 方法A: PWABuilder（もっとも簡単・ブラウザだけで完結）

1. https://www.pwabuilder.com/ を開く
2. 公開したURL（`https://【ユーザー名】.github.io/ring-sizer/`）を入力して「Start」
3. 「Android」パッケージを選択し、内容を確認して生成
4. ダウンロードしたAPK/AABファイルを端末にインストール、またはGoogle Play Consoleにアップロード

### 方法B: Bubblewrap CLI（Googleの公式ツール、CI/GitHub Actionsでも自動化可能）

```bash
npm i -g @bubblewrap/cli
bubblewrap init --manifest https://【ユーザー名】.github.io/ring-sizer/manifest.json
bubblewrap build
```

生成された `app-release-signed.apk` を配布・インストールできます。

## 注意点

- 測定履歴・キャリブレーション・学習補正データはすべて端末のブラウザ（またはTWAのWebView）内に保存され、外部サーバーには送信されません。GitHub Pagesはアプリのコードを配信するだけです。
- アイコンやデザインを変更したい場合は `icons/` を差し替え、`manifest.json` の該当箇所を更新してください。
- 独自ドメインで公開したい場合は、GitHub PagesのSettingsでカスタムドメインを設定できます（TWA化する際はそのドメインを使ってください）。
