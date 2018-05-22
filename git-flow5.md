## [Getting started](https://danielkummer.github.io/git-flow-cheatsheet/index.ja_JP.html#getting_started)

Git flowを開始するには 既存のプロジェクトをカスタマイズします。

★ ★ ★

### 初期化

通常のgitリポジトリ配下に移動した後、下記のコマンドでgit-flow化します。

> git flow init

コマンドのあと対話形式で、いくつかの質問に答えます。 大体はデフォルトの値が推奨されます。

## [Features](https://danielkummer.github.io/git-flow-cheatsheet/index.ja_JP.html#features)

- 通常の開発を行います。
- 基本的には開発者のリポジトリのみに行います。

★ ★ ★

### 開発開始

開発用ブランチは 'develop' ブランチから開始します。開始方法は、

> git flow feature start MYFEATURE

新たな開発用ブランチを'develop'ブランチをベースとして作成し、開発用ブランチにスイッチします。

### 開発終了

開発が終了したらコマンドで以下の操作が行われます。

- MYFEATUREブランチを'develop'にマージします。
- 開発用ブランチを削除します。
- そして、'develop'ブランチにスイッチをします。

> git flow feature finish MYFEATURE

### 開発分をリモートへ

複数人と同じ開発ブランチで作業するときは、 自分の変更分をリモートサーバにプッシュします。

> git flow feature publish MYFEATURE

### 修正分を取り込む

他の人の修正分を自分のローカルにプルします。

> git flow feature pull MYFEATURE

## [Make a release](https://danielkummer.github.io/git-flow-cheatsheet/index.ja_JP.html#release)

- リリースのための準備を行います。
- 軽微なバグフィックスを行ったり、リリースのため作業を行います。

★ ★ ★

### リリース準備開始

リリース作業を開始するには、git flowのreleaseコマンドを使います

> git flow release start RELEASE [BASE]

`[BASE]`はオプションで 'develop'ブランチの特定のCommitのハッシュ値を指定します。指定がない場合はHEADが使われます。

★ ★ ★

'release'ブランチ作成後に修正をプッシュするには、'feature'の時と似たコマンドを使用します:

> git flow release publish RELEASE

('release'リポジトリの修正のトラッキングをすることもできます
`git flow release track RELEASE` )

### リリース準備完了

リリース準備の終了作業は、gitのリポジトリが大きく変化します:

- 'release'ブランチを'master'にマージします。
- 'master'ブランチにリリース用のタグをつけます。
- 'develop'ブランチに'release'ブランチの内容がマージされます。
- 'release'ブランチが削除されます。

> git flow release finish RELEASE

## [Hotfixes](https://danielkummer.github.io/git-flow-cheatsheet/index.ja_JP.html#hotfixes)

- すぐに適用しなければいけないような、緊急の場合に使用します。
- 'master'ブランチのタグから、緊急対応用のブランチを作成します。

★ ★ ★

### 緊急対応の開始

他のgit flowコマンドと似た形で、hotfixを開始します

> git flow hotfix start VERSION [BASENAME]

バージョンの引数は、ホットフィックスリリース名を指定します。 オプションとして開始するベースを指定出来ます。

### 緊急対応の終了

緊急対応の終了作業は、'develop'と'master'のブランチをマージします。加えて、'master'ブランチは緊急対応のタグが付けられます。

> git flow hotfix finish VERSION

## [Commands](https://danielkummer.github.io/git-flow-cheatsheet/index.ja_JP.html#commands)

## Backlog

★ ★ ★

- コマンドのすべてをカバーしているわけではなく、重要なものだけカバーしています。
- もちろん、gitのコマンドは通常通りすべて使用することができます。git flowは単にgitのコマンドの集合です。
- 'support'ブランチの機能はまだベータ版です。それについては言及できません。
- もし翻訳して頂けるなら、統合してもらえると幸いです。

★ ★ ★