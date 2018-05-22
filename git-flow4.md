# git flow × Pull Request を使ってモダンな開発してみた







## はじめに

この記事はgitflow&PullRequestの使い方を解説するというよりも、
使っている上で困ったことや、とまどったことの記録です！(ある程度使い方の流れもかいてますが)
また、gitflowはリリースまでの流れを持っていますが、ここではfeatureブランチ作成〜developブランチマージまでしか触れていません。
導入予定なんだけど、使い方わからんよ〜という人はすでに優秀な記事がたくさんあるので、下のあたりのリンクをたどっていってくださいね〜

## gitflowを使う準備

▼gitflowって何？って人はこちら
[いまさら聞けない、成功するブランチモデルとgit-flowの基礎知識](http://www.atmarkit.co.jp/ait/articles/1311/18/news017.html)

▼git-flowセットアップに関してはこちら
<http://chuross.hateblo.jp/entry/2012/12/30/021415>

▼自分はcentOS(Linux)にセットアップしたのでこれを参考にしました
<http://chuross.hateblo.jp/entry/2012/12/30/021415>

yumでEPEL使える状態にしてから、
`yum install gitflow`
で完了！

## とりあえず必要だったコマンドと困ったこと一覧

単純にgit-flowのコマンドの流れを知りたいならこれを見ればOK!
[git-flow cheatsheet](http://danielkummer.github.io/git-flow-cheatsheet/index.ja_JP.html)

### 開発を始めよう!

まずは普通にクローン
`git clone [githubでコピったURL]`

cloneしたらcdでgit配下まで移動後、
git flowを使い始めます宣言！
`git flow init`
いろいろ聞かれるので基本Enter連打w（デフォルトのままでOK）

### featureをスタートしよう！

早速feature start??
のまえに・・

▶︎developを最新の状態(リモートと同じ状態)にしておきましょう！
※cloneしたては不要かも。でも2回目以降は忘れずに。
`git checkout develop`
`git branch`　→　developブランチになっていることを確認
`git pull origin develop`

▶︎最新状態になったら新しくfeatureを切る！
`git flow feature start [ブランチ名]`

これでdevelopブランチと同じ状態の、featureブランチができあがって、
そのままそのブランチにスイッチします。

### 開発中によく使った便利コマンド

`git status``git add`みたいな定番必須コマンドは除外！
いや、人によっては下も定番なのかもしれないけど、未熟者のわたしには感動もんでした

▶︎`git stash`
ブランチを切り替えたいけど、今編集中のファイルがあって切り替えられない！
そんなときに編集ファイルを一時保存してくれるのがgit stash!
git-flowを使うとbranchをよく切り替えるので、これはまじで便利
使い方：<http://goo.gl/JHNMIo>

▶︎`git reset`
あるコミット履歴までファイルの状態、もしくはコミット状態を戻せるコマンド。定番ちゃ定番。
オプションでリセットの仕方も調整できる。
よく使うのはこのへん
　`git reset HEAD` - addの取り消しに使う
　`git reset --hard HEAD^` - 1つ前のコミットを取り消す(ファイルの状態も)
　　→　後述の`cherry-pick`後の取り消しに使えるが、復元不可なので取り扱い注意
詳しくはこの記事わかりやすいっす
[git-resetは結局何を戻すのか](http://qiita.com/fnobi/items/ec036c1b5d7ee5a8517c)

▶︎`git cherry-pick [コミットID]`
あるコミットを、別のブランチにコピーできるという便利技
間違えて別のブランチで作業しててコミットしちゃったよ！ってときに便利
コピー後は、`git reset --hard HEAD^`でばっちり戻しておきましょー
使い方：<http://goo.gl/ScuyPe>

### ちょっとやっかい git rebase

gitflow × pullrequest を使うにあたって、重要となるのは`git rebase`
(git flow用のコマンドは`git flow feature rebase [ブランチ名]`)
概念がややこしいのですが、この記事がとても素敵に解説してます
[Git rebaseの使い方を解説します - 株式会社LIG](http://liginc.co.jp/web/tool/79390)

▶︎コンフリクトしたとき
ちょっと混乱するのがコンフリクトの仕方。
コンフリクトが起きたときは、
　① `git status`でコンフリクトファイルを確認
　② コンフリクトを解消
　③ `git add .`でファイルを追加
　④ コミットはせず、`git rebase --continue`で続ける

記事で解説があったように、これ、rebase元のコミットを、一つひとつコミットし直してるんです。
`git rebase --continue`
をしたのにまたコンフリクトすることもあります。これ正しい姿！
なので根気よくcontinueし続けて直しましょう〜
（developとfeatureの差が大きくなりすぎて、あまりにコンフリクトが多い場合は
　developからfeatureをもう一回切り直して、手動でマージするのがベターかも）

### feature開発完了！レビューを出そう

featureにバグなし！
developのrebaseも完了している！
次はこのコマンドだ！
▶︎`git flow feature publish [ブランチ名]`
　( = `git push origin feature/[ブランチ名]`)
要はpushですw
これでリモート上にfeatureブランチがめでたくあがるわけです！

※publishはなぜか2回目以降は使えないので、
　一度publishしたあとは、`git push origin feature/[ブランチ名]`の方でpushしましょー

そしてここで初めて登場するのが **Pull Request**!!
プルリクは、言うなればコードレビューの手法の一種
GitHubがそんなステージを用意してくれていますー（詳しくは後述）

## Pull Request(プルリク)って何?

Pull Request ・・　そのまま訳すと、「プルしてね♪」っていうリクエストですね
若干手法がいろいろとあるようなんですが、今回紹介するのはまさにこの「Forkしない方のPullRequest」です。
基本的なやり方はこちらを見てもらえれば！
[GitHub初心者はForkしない方のPull Requestから入門しよう](http://blog.qnyp.com/2013/05/28/pull-request-for-github-beginners/)

プルリクを受けたレビュワーさんは、
　・自分のローカル上にブランチをcheckout
　・コードレビュー(GitHub上)＆動作レビュー
　・レビュー完了後、レビュイーに修正を返す
　・修正完了後、developにマージ
というステップを踏みます

注意すべきは、git-flowと、GitHubのプルリクの仕様が若干かみあってないところ
調整はヒトがするしかない・・！そのあたりを中心に少し解説します。

### レビュイーさんがプルリクを出すとき

・差分元の[master]を[develop]に変更してから出しましょうー
　　→　masterからの差分だと他のブランチの差分コミットまで出てきてしまいます！

### 自分のローカル上に、他人のブランチをチェックアウトするとき

コマンドとしてはこちら
`git flow feature track [ブランチ名]`
( = `git checkout -b feature/[ブランチ名] remotes/origin/feature/[ブランチ名]`)
git checkout -b は新規ブランチの作成コマンドで、
リモート上のブランチ名を指定することで、自動的にリモートのブランチの状態をコピーしてくれます

### 修正完了後、developにマージするとき

GitHub上でのマージ機能がありますが、git-flowの仕様ではマージしてはいけません！(マージ先がmasterブランチしか選べない)
ので、必ずローカル上でfinishします！
`git flow feature finish [ブランチ名]`
によってdevelopにマージ・作業中のブランチは削除され、developブランチにスイッチします！
（通常、developにマージするのはレビュワーですが、これは組織のルールによりますかね）
ローカル上の出来事なので、最後にリモートのdevelopと同期して完了！
`git push origin develop`

ちなみにリポジトリの削除もローカル上の出来事なので、
リモート上のブランチの状態を変えるには、直接削除してあげなければいけません
せっかくの自動削除機能がちょっともったいないと思う・・

### マージ後は？

また新たなfeatureを切って開発し、pullrequestを繰り返します
サービスが形になった時点で、releaseブランチを切りますが、
ここではreleaseブランチ後の説明は割愛します！

## やってみての所感

git-flowは非常に美しい開発フローだなと感じます。
PullRequestと組み合わせることで、効率的かつわかりやすい開発が可能なハズ・・です。

今回のケースは下記のような環境もあり、苦労した点も多々ありました・・！
　・gitにあまり慣れていない開発メンバーであること
　・初期開発であること

苦労した点は、具体的には下記
　・初期開発ゆえ、featureを切る単位が難しい
　　　→　要スタンス合わせ。大きすぎても小さすぎても大変だが、最初は大きくなりがち。小さめにこまめにマージするのがベター。
　・コンフリクトの多発
　　　→　rebaseのタイミングによって(developとの差分が生まれすぎた場合)はbranchの切り直しも検討が必要。
　・branchの扱いを理解するまでの学習コスト
　　　→　branch切り替えの概念理解と、切り替え時のローカル環境⇄ローカルサーバ環境のやりとりで多少難あり。
　・レビュワー不足によるマージまでの時間差
　　　→　developが最新ではなく、develop+最新featureが必要な状態が続いてしまうのはよくない状況。レビュワーのマージが遅れると発生しがち。

初心者に少々きついものもありましたが、勉強になりました。

以上、長々とかいてきましたがgitflow + PullRequestの少しばかり混み入ったお話でした！
他サイトさんの解説が全然優秀なので、補足とプラスα情報として扱っていただければと思いますー！