# sizuku-taiken.com

琉球ガラス工房 雫の予約・カタログへの入り口となるゲートウェイページです。
静的HTML（index.html / cats.html）のみで構成されており、サーバーサイドの処理はありません。

## ローカルで確認する

```bash
npx serve .
```

を実行し、表示されたURL（例: http://localhost:3000）をブラウザで開いてください。

## 1. GitHubにアップロードする

1. https://github.com/new で新しいリポジトリを作成する（例: `sizuku-taiken-site`）。Public / Private どちらでも構いません。
2. このフォルダの中身一式をリポジトリにアップロードします。ターミナルが使える場合は次のコマンドで一気にプッシュできます。

```bash
cd sizuku-taiken-site
git init
git add .
git commit -m "Initial commit: sizuku-taiken.com gateway page"
git branch -M main
git remote add origin https://github.com/<あなたのユーザー名>/sizuku-taiken-site.git
git push -u origin main
```

ターミナルを使わない場合は、GitHubのリポジトリ画面から「Add file → Upload files」でこのフォルダの中身をドラッグ＆ドロップしてアップロードすることもできます（`cats`フォルダも含めて全ファイル）。

## 2. Railwayにデプロイする

1. https://railway.com/new を開く
2. 「Deploy from GitHub repo」を選択し、Railwayに自分のGitHubアカウントを連携する
3. 先ほど作成した `sizuku-taiken-site` リポジトリを選択する
4. このリポジトリには `package.json` が含まれているので、Railwayが自動的にNode.js環境と判断し、`npm install` → `npm start`（`serve` コマンドで静的ファイルを配信）を実行します。特別な設定は不要です
5. デプロイが完了すると `https://xxxxx.up.railway.app` のようなURLが発行されるので、まずそこで正しく表示されるか確認してください

## 3. 独自ドメイン（sizuku-taiken.com）を設定する

### Railway側の設定

1. Railwayのプロジェクト画面 → 対象サービスの `Settings` タブ → `Networking` セクションを開く
2. `+ Custom Domain` をクリックし、`sizuku-taiken.com` を入力する
3. Railwayが **CNAMEレコード** と **TXTレコード** を発行するので、両方をメモする（TXTレコードがないと、CNAMEが正しく反映されてもサイトが404になります）
4. 同様に `www.sizuku-taiken.com` も追加しておくと安心です（`www`付きでアクセスされた場合の受け皿になります）

### ドメイン管理会社（DNS）側の設定

ルートドメイン（`sizuku-taiken.com` のように `www` が付かない形）は仕組み上、通常のCNAMEレコードを直接設定できません。お使いのDNS事業者によって対応方法が異なります。

- **Cloudflareを使っている場合**：ルートドメインにも通常のCNAMEレコードをそのまま設定できます（Cloudflareが内部で自動的に処理してくれます）
- **Namecheapを使っている場合**：同様にCNAMEレコードをそのまま設定可能です
- **DNSimpleを使っている場合**：「ALIASレコード」を使用します
- **bunny.netを使っている場合**：「ANAMEレコード」を使用します
- **上記のいずれにも対応していない場合**：ドメインのネームサーバーをCloudflareに切り替え、Cloudflare側でCNAMEフラット化を使う方法が確実です

`www.sizuku-taiken.com` の方はどのDNS事業者でも通常のCNAMEレコードで設定できます。

設定後、DNSの反映には数分〜最大72時間かかることがあります。反映されるとRailway側で自動的にSSL証明書（https）が発行されます。

### 補足：reserve / catalog サブドメインについて

`reserve.sizuku-taiken.com` と `catalog.sizuku-taiken.com` は今回のRailwayデプロイとは別のサービスとして運用されている想定です。このリポジトリはあくまで `sizuku-taiken.com`（ルートドメイン）のゲートウェイページのみを対象としています。既存のreserve/catalogのDNS設定には影響しません。

## ファイル構成

- `index.html` — トップページ
- `cats.html` — 看板猫の写真集ページ
- `cats/` — 看板猫の写真（10枚）
- `logo-mark.png` — サイト共通ロゴ（ヘッダー・フッター・ヒーロー等）
- `logo-drop.png` — 「工房について」セクションの装飾用アイコン
- `favicon.png` — ファビコン
- `og-image.png` — SNSシェア用OGP画像
- `robots.txt` / `sitemap.xml` — SEO用ファイル
- `package.json` / `railway.json` — Railwayで静的サイトを配信するための設定
