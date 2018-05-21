# git-flowを試す





## git-flow



参考サイト:
<http://www.atmarkit.co.jp/ait/articles/1311/18/news017.html>

gitのブランチ管理ツール。
それぞれの役割を持った、以下の5つのブランチで管理する。

- develop: 開発用
- feature: 機能実装、バグフィックス用
- release: リリース準備用
- master: リリースしたソースコード用
- hotfix: 緊急のバグフィックス用

## インストール

参考サイト:
<http://www.atmarkit.co.jp/ait/articles/1311/28/news042.html>

homebrewなら以下の通り。

```
brew install git-flow
```

※その他の場合: <https://github.com/nvie/gitflow/wiki/Mac-OS-X>
Source Tree などgit-flowに対応したGUIを使う方法もある。

## git-flowを使った作業用リポジトリを作成

試しにgit-flowを使った作業用リポジトリを作成してみる。
initはmasterブランチでした方がいいかも。

```
$ git clone https://github.com/note109/git-flowTest.git
$ cd git-floeTest
$ git flow init -d
```

-dオプションをつけると5つのブランチ名がデフォルト名で命名される。

ブランチを作成したら、ブランチを含めてpushする。（--allオプション）

```
$ git push --all
```

ブラウザでgithubを見ると、masterブランチとdevelopブランチが出来たことを確認できた。

共同開発者もこれと同様の操作をすることで、git-flowを使ってチームで開発を進められる。

## ブランチを操作

参考サイト:
<http://www.atmarkit.co.jp/ait/articles/1401/06/news013.html>

作ったリポジトリで実際にブランチを操作してみる。

# ブランチを作成

start コマンドを使う。

```
$ git flow feature start <ブランチ名>
```

feature ブランチ以外も同様に作成可能。

# ブランチを共有

publish コマンドを使う。

リモートにブランチをプッシュする

```
$ git flow feature publish myfeat
```

ブラウザでgithubを見ると、feature/myfeatというブランチが作成されていることが確認できた。

# ブランチを取得

track コマンドを使う。

```
$ git flow feature track myfeat
```

試しにもう一つテストリポジトリをcloneしてきて実行してみると、feature/myfeatブランチが取得できた。

# コミットのプッシュ

gitのpush コマンドを使う。

```
$ git push
```

# コミットの取り込み

共有リポジトリ上の変更をローカルリポジトリに取り込む。

```
$ git flow feature pull origin
Pulled origin's changes into feature/myfeat.
$ git flow feature rebase
Will try to rebase 'myfeat'...
Current branch feature/myfeat is up to date.
```

共有リポジトリに無いブランチにcheckoutしているときはできない。

```
fatal: Couldn't find remote ref feature/myfeat2
Unexpected end of command stream
Failed to pull from remote 'origin'.
```

# ブランチでの作業を終了

finish コマンドを使う。

```
$ git flow feature finish myfeat
```

featureブランチを終了する場合、developブランチへマージされる。

ブランチの変更を含めてpushする。（--allオプション）

```
$ git push --all
```

# masterブランチへ反映

参考: <http://danielkummer.github.io/git-flow-cheatsheet/index.ja_JP.html>

developブランチからリリース用ブランチをstartさせる

```
$ git flow release start RELEASE <BASE>
```

を指定しなければHEADが使われる。

リリースブランチで変更がある場合はpublishしとく。

```
$ git flow release publish RELEASE
```

リリースブランチを終了させる。

```
$ git flow release finish RELEASE
```

このあとマスターブランチでpushすればたぶんokっぽい

## 追記: 実際に使った感じ

developブランチからfeatureブランチを作る → publishしてgithubでプルリクエストを出す
という流れが自分が日々やってる開発では主体になってる。

releaseブランチとか分からなくても、これだけ出来れば僕の立場的には何とかなってます。

```
$ git pull origin develop
```

↓

```
$ git flow feature start BRANCH_NAME
```

↓

```
$ git flow feature publish BRANCH_NAME
```

↓

githubのWebサイトでdevelopブランチにプルリクエストを出す。