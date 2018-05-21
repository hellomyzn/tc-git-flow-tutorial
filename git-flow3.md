# Git Flowやってみた





## 環境

Mac OSX Yosemite
git 2.1
git flow 0.4

# 0. 事前準備

## git cloneでローカルリポジトリ作成

httpsでクローンする場合

```
git clone https://github.com/user_account/repository_name.git
```

sshでクローンする場合

```
git clone git@github.com:user_account/repositry_name.git
```

httpsでクローンしてこんなエラーが出た場合は、

```
Cloning into 'repository_name'...
fatal: unable to access 'https://github.com/XXXXXXX/git-study.git/': SSLRead()
return error -9806
```

sshの方でやってみよう。

githubの[ヘルプ](https://help.github.com/articles/checking-for-existing-ssh-keys/)を参考にsshキーを登録するとよい。

## Homebrewをインストールしておく

<http://brew.sh/index_ja.html>参照。

## git-flowのインストール

以下のコマンドでgit-flowをインストールする。

```
brew install git-flow
```

# 1. GitコマンドでGit flow

GitコマンドだけでGit flowぽくやってみた。

## masterのクローン

masterをリモートからクローンし、ローカルのmasterブランチを作成する

```
git checkout -b master origin/master
```

同様にクローンしたいブランチがあれば、リモートのブランチを確認し、上と同じコマンドでクローンする。

```
git branch -r
git checkout -b local_branch_name origin/branch_name
```

## developブランチの作成

developの作成

```
git branch develop
git push -u origin develop
```

pushした時パスワードを求められるのでgithubアカウントのパスワードを入力する。

同様にrelease、hotfix、featureを作成する。←作成しなくてもいい。

```
git branch ブランチ名
git push -u origin ブランチ名
```

## developからfeatureブランチを作成

1.featureブランチを作成する

```
git checkout -b featureブランチ名 develop
```

git checkoutはブランチを切り替える。
-bをつけるとブランチがない場合に新規作成して切り替える。
第2引数に親ブランチを指定することもできる。
featureはdevelopから作成するので上記のコマンドとなる。

2.featureを空コミットする

```
git commit --allow-empty
```

3.リモートにpushする

```
git push -u origin featureブランチ名
```

4.git hubのブラウザでプルリクエストを作成。

## 変更を追加・コミットする

1.git statusで変更を確認、git addでステージにファイルを追加、そしてコミットする。

```
git status
git add <some-file>
git commit
```

2.リモートへpushする。

```
git push
```

または

```
git push -u origin featureブランチ名
```

3.ブラウザでプルリクをレビューし、問題無ければマージしてfeatureブランチを削除して終了

リリースやホットフィックスの開始も基本的には同じ。

## リモートの変更をローカルに同期する

```
git fetch origin
```

## リモートの変更を取り込む

リモートからカレントのブランチにpullします。

```
git pull
```

カレントがfeatureの時に別のブランチの変更をfeatureにマージします。

```
git merge ブランチ名
```

マージでコンフリクトの対応が完了したら、

```
git add file_name
git commit -m 'confrict fixed' #コメントは競合の解決がわかれば何でもいい
```

ここまではウォーミングアップ。
ここからが本番

# 2. Git FlowコマンドでGit Flow

コマンドのみでgit-flowをやってみよう。

## 初期化

ローカルのリポジトリに移動して初期化します。

```
cd repository_name
git flow init -d
```

すると、こんな感じで

```
Using default branch names.

Which branch should be used for bringing forth production releases?
   - develop
   - master
Branch name for production releases: [master] 

Which branch should be used for integration of the "next release"?
   - develop
Branch name for "next release" development: [develop] 

How to name your supporting branch prefixes?
Feature branches? [feature/] 
Release branches? [release/] 
Hotfix branches? [hotfix/] 
Support branches? [support/] 
Version tag prefix? [] 
```

各ブランチが作成される。

初期化をやり直したいときは

```
git flow init -f
```

そして、push

```
git push --all
```

## featureブランチの作成

1.featureブランチ作成

```
git flow feature start myfeature
```

2.プッシュして共有

```
git flow feature publish myfeature
```

3.ブラウザでプルリク作成

## 別のfeatureをローカルに取り込むには

このコマンドで別のfeatureをクローンする。
ローカルで動作確認やレビューを実施したい場合に使おう。

```
git flow feature track feature_name
```

## featureの終了

finishコマンドでfeatureからdevelopへマージし、ローカルのfeatureブランチを削除する。
pushしてリモートを最新化する。

```
git flow feature finish myfeature
git push --all
```

## リリースブランチ作成

リリースブランチを作成する
コマンドはfeatureの部分をreleaseに変えるだけ

```
git flow release start myrelease
```

リリースブランチに修正がある場合はプルリクを作成する。

```
git flow release publish myrelease
```

リリースブランチの終了とともに、masterにマージされる。
デフォルトだとmyreleaseがタグ名となる。

```
git flow release finish myrelease
```

ブランチがmasterに切り替わるのでpushし、masterを最新化する。

```
git push --all
```

タグをリモートにpushする

```
git push origin --tags
```

## ホットフィックス対応

バグ等で緊急リリースが必要な場合ホットフィックスを作成する。
VERSIONにはホットフィックスのリリース名を指定。
リビジョンを1つ上げる感じかと。
BASENAMEはたぶんデフォルトがmasterかと思われる。

```
git flow hotfix start VERSION [BASENAME]
```

対応が完了したらホットフィックスを終了する。

```
git flow hotfix finish VERSION
```

ホットフィックスの対応をするにあたり、リリースタグの一覧はこのコマンドで確認できる。

```
git tag
```

## まとめ

- Git flowも基本的にはGitコマンドでやることと大して変わらない。
- Gitについて理解が深まった。
- GUIのツールは偉大だ。
- プルリク作成タイミングやfeatureの終了タイミング、developへのマージタイミングはプロジェクトによって多少違うかもしれない。レビューのやり方とか開発フローによって決まる気がしなくもない。