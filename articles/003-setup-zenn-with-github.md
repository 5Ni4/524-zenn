---
title: "ZennはVS Codeで書け！Git連携のすゝめ"
emoji: "🐈"
type: "tech"
topics: ["zenn", "git", "github", "vscode", "初心者"]
published: false
---

# Zennに投稿するならVS Codeで書け！ Gitリポジトリ連携のすゝめ

## きっかけ

Zennでの記事投稿を始めたての筆者は、VScodeにMarkDownの拡張機能をいくつか入れて執筆環境としていた。

すでに2本公開しているがスラッグ設定を見落としており、ランダムな文字列になってしまった。しかも後から変更できないのが痛い（リンク変わっちゃうので当たり前）。

ま、まあスラッグより？？記事の内容が100倍大事だし？？細かいこと気にしても仕方ないよな？？？と思っていたところ、 **ClaudeからZenn CLIを紹介**された。

導入するメリットは多数あるとのことで、**MDで執筆した記事ファイルをGitHubのリポジトリで管理でき、スラッグや投稿日時もよしなに指定できる**らしい。

固定観念で、**リポジトリはソースコードを管理する場所**っていうイメージしか無かったので、ドキュメント管理に用いるという発想は目からウロコだった。確かに差分管理できるしめっちゃ便利そう。

そんなこんなで、ウキウキしながら筆者は Zenn CLI を導入したのであった。

## こんな人向けだよ

- Zennで記事投稿を始めたばかり（筆者も同じく）
- ブラウザで執筆&投稿は効率が悪いと感じている
- 使い慣れたVS Codeで執筆作業をしたい

## 設定手順

設定箇所が複数あるのでちょっとややこしいけど、今回もねこさんと見ていこう🐈

### 前提条件

- GitHubアカウントがある
- Gitの基本的なコマンドがわかる（clone, add, commit, push）
- Node.jsがインストールされてる

※ Node.jsがない人は[公式サイト](https://nodejs.org/)からLTS版をダウンロードだ！

### 1. GitHubでリポジトリを作成

[GitHubにログイン](https://github.com/)し、Repositoriesメニューを開いて新規作成。

![タブメニューからリポジトリを選択](/images/003/open-repositories-page.png =600x)

※ 画像はリポジトリ作成済みの場合の画面

#### 設定項目

1. **リポジトリ名**: `zenn-contents` とか
2. **説明**: 書きたかったら書く（任意）
3. **公開範囲**： `Public`（推奨）
4. **ADD README** 何のこっちゃわからん人は`OFF`でヨシ（後から作れる）
5. **Add .gitignore**: 必ず`Node`を選択⚠️
6. **Add license**: `No license` で問題ない
7. 「**Create repository**」で作成！

![リポジトリ新規作成ダイアログ](/images/003/create-new-repository.png =400x)

:::message
プライベートリポジトリで始めたい場合は、GitHub ProまたはOrganizationが必要。
パブリックでも`published: false`の記事は公開されないので問題はない。
:::

### 2. Zennと連携

GitHub側の準備ができたらZenn側をすすめていく。
下準備はできている状態だから、もう少しの辛抱だよ〜〜🌱

#### 手順

1. [Zennのダッシュボード](https://zenn.dev/dashboard/deploys)を開く
2. GitHub連携メニューを開き、「リポジトリを連携する」をクリック
![GitHubとZennの連携開始](/images/003/start-github-connection.png =520x)
3. 「連携へ進む」をクリック
![GitHubとZennの連携開始](/images/003/start-conntection-setting.png =320x)
4. パスワードやアプリなど、任意の方法でGitHub認証する
![GitHubの認証](/images/003/github-conrifm-access.png =340x)
5. 作成したリポジトリを選択
![GitHubの認証](/images/003/authorize-zenn-connect.png =400x)
6. 連携完了！✌️
![GitHub連携完了](/images/003/done-github-connection.png =600x)
これで**GitHubにpushすると自動的にZennに反映される**ようになる。

### 3. ローカルで環境構築

VSCodeのターミナルを開いて以下を実行

```bash
# リポジトリをクローン
git clone https://github.com/あなたのユーザー名/リポジトリ名.git
cd リポジトリ名

# Zenn CLIをインストール
npm init -y
npm install zenn-cli

# 初期化
npx zenn init
```

結果は画像の通りになるはず（※ スクリーンショットは2025-11-29時点）

![コマンド実行後のディレクトリ構成](/images/003/done-command-zenn-cli.png =560x)

これで設定が完了、おつかれさまです〜〜〜🍵

## 記事を書いてみよう

### 1. ファイル作成

以下のコマンドを実行し、記事の元となるマークダウンファイルを作成する

```bash
# スラッグを指定して記事作成
npx zenn new:article --slug my-first-article
```

:::message alert
**重要：スラッグは必ず指定すること！**

指定しないとランダムな文字列（例：`a1b2c3d4e5f6`）になってしまう。
公開後は変更できないので先に決めておこう。
:::

### 2. いざ、執筆

先ほど作成した記事が`articles`に生まれているので、そちらに本文を書いていこう。
マークダウンのための拡張機能がいくつかあるので、VScodeに入れてから作業開始するとよい。

#### フロントマターの例

```yaml
---
title: "記事のタイトル"
emoji: "🐈"
type: "tech"  # tech or idea
topics: ["zenn", "git", "github"]  # 最大5つ
published: false  # 公開する時は true
published_at: 2025-11-26 09:00  # 自動公開の日時（任意）
---
```

#### プレビューを見る方法

以下コマンドを実行。

```bash
npx zenn preview
```

ブラウザで`http://localhost:8000`にアクセスすると、Zennで公開した場合の記事の見え方を確認することができる。
ブラウザのリロードは必要なく、保存したらすぐ反映されるのがアツい。

### 3. 完成した記事をGitHubにpush

執筆したファイルをステージしたのちにコミット。問題なさそうならプッシュ。

```bash
git add .
git commit -m "記事を追加: 初めてのGit連携"
git push origin main
```

- `published: true`の場合、**プッシュした瞬間に記事が投稿される**。
- `published: false`の場合は、下書きとしてZennに反映される。
  `published_at`が記載されている場合は、その日時に自動公開される。

## まとめ

Zenn CLI を使うとこれが良い！！

- **バージョン管理**： GitHubのリポジトリと連携することで記事を過去の内容にすぐ戻せて安全🪴
- **日時指定公開**： 公開したいタイミングにZennにログインして作業する必要がない🦥
- **VSCodeで書ける**： エンジニアみんなの実家、快適な執筆環境を展開だ〜🚀

初期設定はちょっと手間だけど、それに見合うメリットがあるのでぜひやってみてね

## 参考：公式による記事

詳しく記載されているので、もっと深く知りたいひとは覗いてみよう👀

- [Zenn CLIで記事・本を管理する方法](https://zenn.dev/zenn/articles/zenn-cli-guide)
- [GitHubリポジトリでZennのコンテンツを管理する](https://zenn.dev/zenn/articles/connect-to-github)
