# Git-flowって何？



[Git](https://qiita.com/tags/Git)[GitHub](https://qiita.com/tags/GitHub)

 この記事は最終更新日から1年以上が経過しています。

git-flowとは、プラグイン(ツール)のことです。。
Vincent Driessen氏がブログに書いた"A successful Git branching model" というブランチモデルの導入を簡単にする git プラグインである。
参考資料：
・　<http://hm-solution.jp/lifehack/post2475.html>
・　<http://d.hatena.ne.jp/Yamashiro0217/20120903/1346640190>



# Git-flowイメージと各ブランチの役割

[![git-model@2x.png](https://camo.qiitausercontent.com/bbb6ba70f058f0d10c8f7a769ae8fd089dc7684e/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f35333330392f30363134303132312d613062362d343237662d633134392d3638353863313439373338652e706e67)](https://camo.qiitausercontent.com/bbb6ba70f058f0d10c8f7a769ae8fd089dc7684e/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f35333330392f30363134303132312d613062362d343237662d633134392d3638353863313439373338652e706e67)

#### master:

プロダクトとしてリリースするためのブランチ。リリースしたらタグ付けする。

#### develop:

開発ブランチ。コードが安定し、リリース準備ができたら master へマージする。リリース前はこのブランチが最新バージョンとなる。

#### feature branches:

機能の追加。 develop から分岐し、 develop にマージする。

#### hotfixes:

リリース後のクリティカルなバグフィックスなど、 現在のプロダクトのバージョンに対する変更用。 master から分岐し、 master にマージし、タグをつける。次に develop にマージする。

#### release branches:

プロダクトリリースの準備。 機能の追加やマイナーなバグフィックスとは独立させることで、 リリース時に含めるコードを綺麗な状態に保つ（機能追加中で未使用のコードなどを含まないようにする）ことができる。 develop ブランチにリリース予定の機能やバグフィックスがほぼ反映した状態で develop から分岐する。 リリース準備が整ったら, master にマージし、タグをつける。次に develop にマージする。

## 1.ローカルリポジトリにクローンを作成する。

共有リポジトリを利用して開発を進める場合、クローン(clone)して作業ディレクトリをローカルに作成します。このクローンはサーバが保持しているデータをほぼすべてローカルにコピーします。これはプロジェクトのすべてのファイルのすべての履歴が手元にコピーされることを意味しています。

作業ディレクトリを作成するコマンドは下記を実行します。
※clone以下はGitHubのhttps等のURLを入れて下さい。
`git clone https://github.com/.git`

これで共有リポジトリからローカルに作業ディレクトリを作成します。
※git clone の後のURLをHTTPSにしないと、下記のエラーになります。
(SSHの場合はエラーになる）

実行結果（エラー）

```
[vagrant@localhost ~]$ git clone git@github.com:SoneKosuke/pullreq.git
Initialized empty Git repository in /home/vagrant/pullreq/.git/
Permission denied (publickey).
fatal: The remote end hung up unexpectedly
```

成功の場合は下記の様な表示になる。

実行結果（成功）

```
[vagrant@localhost ~]$ git clone https://github.com/SoneKosuke/pullreq.git
Initialized empty Git repository in /home/vagrant/pullreq/.git/
remote: Counting objects: 7, done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 7 (delta 0), reused 3 (delta 0)
Unpacking objects: 100% (7/7), done.
```





## 2.Git-flowの初期化を行います。

下記のコマンドを実行すると、ローカルリポジトリの初期化ができます。
実行すると、ブランチの名前などを聞かれますが、基本でフォルトでOKです。
全て`Enter`で！

```
git flow init
```

実行後、branchの状態を確認しましょう。

```
git branch
```

実行結果は下記の様になっています。

実行結果

```
* develop
  master
```

※developブランチが無い場合は、リモートリポジトリに一切データが無い場合です。
developブランチを切る場合は、下記コマンドを実行して下さい。

```
git push origin develop
```





## 3.機能の作成を行うためにFeature branchを切る

この章は、開発チームの誰か一人が行えばOKです。

新しい機能をつける前に、Feature branchをきります。
開発は基本Feature branch上で行います。
ここでは、新しいbranch名を「top」としました。

```
git flow feature start top
```

実行結果

```
Switched to a new branch 'feature/top'

Summary of actions:
- A new branch 'feature/top' was created, based on 'develop'
- You are now on branch 'feature/top'

Now, start committing on your feature. When done, use:

     git flow feature finish top
```

成功したか確認しましょう。

```
git branch
```

develop branchからcheckoutして、feature/topが作成＆checkinされていれば成功です。

実行結果

```
  develop
* feature/top
  master
```

上記コマンドで作成されたブランチは、個人のローカル上に作成しています。
その為、GitHubには反映されていません。
反映させるには、下記コマンドを実行して`push`を行う必要があります。

```
git flow feature publish top
```

実行結果

```
Already on 'feature/top'

Summary of actions:
- A new remote branch 'feature/top' was created
- The local branch 'feature/top' was configured to track the remote branch
- You are now on branch 'feature/top'
```

Pushできたかは、GitHub上で確認できます。
このブランチを使って開発して行きましょう。

[![スクリーンショット 2014-11-02 22.57.35.png](https://camo.qiitausercontent.com/378478acc216b621b1c7c376492a4cc980c0a758/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f35333330392f66636134643637312d323033322d306536662d393735312d3234616336656430666366342e706e67)](https://camo.qiitausercontent.com/378478acc216b621b1c7c376492a4cc980c0a758/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f35333330392f66636134643637312d323033322d306536662d393735312d3234616336656430666366342e706e67)





## 4.開発作業開始。Pullを行う。

作業者は、リモートリポジトリの最新データを使用する必要があります。
（コンフリクトが起きるため）
下記のコマンドでモートリポジトリの最新データをPullしましょう。

```
git flow feature pull origin top
```

実行結果

```
Pulled origin's changes into feature/top.
```

この作業でリモートにあるtopブランチを取得し、それを元にしてローカルにfeature/topブランチが作成されます。
さぁ、開発作業を行いましょう。

------





## 5.開発作業完了。add,commit,push

開発作業がある程度完了して、Pushできる状態になったら、
addとcommitを行います。
下記のコマンドを実行すると、addとcommit両方を同時に実行できます。
"hogehoge"はタグです。変更内容を分かり易く記載して下さい。

```
git commit -am "hogehoge"
```

個別で実行する場合は、下記のコマンド。
「hagehage」はファイル名。"hogehoge"はタグです。

```
git add hagehage.php
git commit -m "hogehoge"        
```

addとcommitが完了したら、pushを行います。
pushを行うと、ローカルリポジトリのデータをGitHubにあげる事ができます。
編集を行った、feature/topブランチをpushします。

```
 git push origin feature/top
```

下記の様な実行結果がでれば成功です。
また、pushできたかは、GitHubでも確認できます。

実行結果

```
Counting objects: 4, done.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 285 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://SoneKosuke@github.com/SoneKosuke/git-flow-test.git
   1acc403..1c5c6ad  feature/top -> feature/top
```

rejectされてpushができない場合。rebaseする必要があります。
他のメンバーが先に編集をpushしたため、fast-forwardでないからです。





## 6.RejectされてPushできない場合

Rebaseを行います。
Rebaseを行うことで、自身の変更を基準にして、branchにマージできます。
下記のLigの記事が分かり易かったので、分からない方は参考にしてください。
<http://liginc.co.jp/web/tool/79390#m1>

Rebaseは下記のコマンドで実行できます。

```
 git pull --rebase origin feature/top
```

pull --rebaseを行うとコンフリクトする事があります。
同じ行を別の内容に変更していたため競合が発生したようです。
競合のあった箇所には、Gitが差分を挿入しています。

この場合は、手動で変更データを変更する必要があります。
変更すべき点は、各メンバーと連携をとりながら変更します。

コンフリクトを解消したら、git addしてgit rebase --continueします。

```
git add fizzbuzz.php 
git rebase --continue
```

rebaseが完了したら、pushを行い作業は完了です。





## 7.GitHub上で、Pull Requestを作成する

GitHub上に作成したpullreqのリポジトリページを開くと、pushしたupdate-readmeブランチに関する情報が表示されているはずです。そこに表示されているCompare & pull requestボタンを押すと、Pull Request作成ページに移動できます。
[![スクリーンショット 2014-11-01 23.23.09.png](https://camo.qiitausercontent.com/0933f98bd317cddffc1f401769a9702af82a6967/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f35333330392f38313737623862622d343437392d393232632d663963372d3233653537636636643430372e706e67)](https://camo.qiitausercontent.com/0933f98bd317cddffc1f401769a9702af82a6967/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f35333330392f38313737623862622d343437392d393232632d663963372d3233653537636636643430372e706e67)

Pull Request作成ページでは、リクエストを送信する相手（通常はリポジトリの管理者）に、どういった変更を加えたのかを説明する内容を記入します。

[![スクリーンショット 2014-11-01 23.25.23.png](https://camo.qiitausercontent.com/b599605f1b1a3aa6b4ba7f3feb1bbf772ddf84d9/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f35333330392f33626138623832612d626638662d303764312d306634322d3937653931343364653933392e706e67)](https://camo.qiitausercontent.com/b599605f1b1a3aa6b4ba7f3feb1bbf772ddf84d9/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f35333330392f33626138623832612d626638662d303764312d306634322d3937653931343364653933392e706e67)

説明文を記入できたら、画面下部に表示されている実際のコミット内容もチェックして、誤りがないかをチェックします。大丈夫そうなら「Create pull request」ボタンを押して、Pull Requestを送信します。

これでPull Requestが送信されました。送信後のページでは、今作成したPull RequestがOpen状態になって表示されていると思います。

[![スクリーンショット 2014-11-01 23.26.10.png](https://camo.qiitausercontent.com/3054afed0d5b1c924f28118e895f40e76a482eb6/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f35333330392f36323034613165612d373831342d623265662d386265392d6538376131643135356630612e706e67)](https://camo.qiitausercontent.com/3054afed0d5b1c924f28118e895f40e76a482eb6/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f35333330392f36323034613165612d373831342d623265662d386265392d6538376131643135356630612e706e67)





## 8. GitHub上で、Pull Requestをマージする

Pull Requestの変更点が問題なければ、マージします。
マージとブランチ削除はGit上でも可能ですが、今回はGitHubで実行します。
github上でマージする場合は、「Merge pull request」→「Confirm Merge」ボタンを押すだけです。
[![スクリーンショット 2014-11-01 23.28.59.png](https://camo.qiitausercontent.com/ec1c6a0aae1971b2ba45ba90d7f79d0928630fe2/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f35333330392f34646538633631632d343830372d653638342d343339612d3938323561613362333439612e706e67)](https://camo.qiitausercontent.com/ec1c6a0aae1971b2ba45ba90d7f79d0928630fe2/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f35333330392f34646538633631632d343830372d653638342d343339612d3938323561613362333439612e706e67)

[![スクリーンショット 2014-11-01 23.32.34.png](https://camo.qiitausercontent.com/8a8825e82765e28add6fee59564cbc483e4e9cfd/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f35333330392f34383138646133622d633233382d656233352d333665302d3732366261366439343130312e706e67)](https://camo.qiitausercontent.com/8a8825e82765e28add6fee59564cbc483e4e9cfd/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f35333330392f34383138646133622d633233382d656233352d333665302d3732366261366439343130312e706e67)

マージが完了した後は、ブランチを削除しましょう。
[![スクリーンショット 2014-11-01 23.34.14.png](https://camo.qiitausercontent.com/52fc3f95c7bc3a78de37a79b8cc93f9d226d3a16/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f35333330392f33633861666139362d613564302d633035382d646336322d3763303066646464636262372e706e67)](https://camo.qiitausercontent.com/52fc3f95c7bc3a78de37a79b8cc93f9d226d3a16/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f35333330392f33633861666139362d613564302d633035382d646336322d3763303066646464636262372e706e67)

以上でおしまい。