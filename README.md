##Gitのアレ
http://marklodato.github.io/visual-git-guide/index-ja.html

###何やねん
とりあえず最低限のコマンドに触れ、職場でGitを使用して作業できる人材を即席培養します

###いや、Gitって何やねん
・Gitは「ギット」と読むらしい  
・Gitはソース管理、バージョン管理してくれるらしい  
・Gitはなんか他の人と連携できて便利らしい  
・ナウい  
みたいな認識でおｋです

###用語
####ソース
納品物、成果物を保存したファイル  
たとえばプログラマの場合はプログラムソース、営業さんなら報告書.xls等でしょうか  
間違って削除してしまったり、よく判らない状態で保存してしまったら困るファイルです

####バージョン管理
Gitは過去のファイルの状態を記憶して管理しておいてくれます  
3日前のファイル、１年前のファイル、後からそれを取り出せます  
要所要所のファイルを記憶して管理していてくれる機能をバージョン管理といいます  
要所って何やねん、といいますと、作業が一段落した時かもしれませんし、終業時かもしれません。決めるのは自分です

####リポジトリ(repository)
Gitが情報を蓄える場所です  
どんなファイルを登録したか、どんな歴史がかつて存在したのか、等、全てが保存されます  
単なる .git という名前のディレクトリです。内部がどうなっているのか、通常は気にする必要はありません  
リポジトリが置かれているディレクトリ配下のツリーがそのリポジトリの管理範囲です**[画像]**  

つまり、今現在のカレントディレクトリがどこか、で使用するリポジトリが変わります  
今からいくつかのコマンドを入力していきますが、カレントディレクトリにリポジトリが見つからない場合、Gitは自動でディレクトリを遡り、リポジトリを探しに行ってくれます  
Gitはリポジトリと連携して、依頼されたコマンドを解釈、実行してくれるわけです

###とりあえずローカル（ひとり）で使ってみる
####インスコ
ぐぐれ！

####初期設定
```
git config --global user.name mokoaki
git config --global user.email mokoriso@gmail.com
```

Gitは誰がその作業をしたか全部記録しようとします。名前とメアドです  
なので、最初に教えておく必要があります
```
git config --global user.name homoaki

# あっ、間違ったわ

git config --global user.name mokoaki
```
という感じに、いつでも変更できます。  
名前は気楽にハンドルネームとかで登録すればいいかと思います  
これらの設定は、登録し直すまでずっと有効です。

あと、この設定をいれておくと、表示がカラフルになって見やすいのでオススメですよ  
```
git config --global color.ui true
```

####config、は設定のコマンドなんだろうけど、 --global って何？
gitの設定は
- 今ログインしている環境全体 (--system) /etc/gitconfig
- 今ログインしているユーザ (--grobal) ~/.gitconfig
- リポジトリ (--local) .git/config

の３箇所に保存されます。  
上から順番に読み込まれ、重複したら設定は上書きされます。  
つまり、上の3つで言うと、下に書いた設定が優先されます

とりあえず --grobal を使っておけば困る事はありません

####とりあえずいじってみましょう
とりあえず、Gitにファイルを登録して確認するくらいまでをちょっと触ってみましょう
```
> mkdir git_test
# とりあえずテスト用にディレクトリを作りましょう

> cd git_test
# そこに移動します

> git init
# Gitのリポジトリを作成します。このコマンドの後は、このディレクトリ配下がGit管理下になります

> ls -la (Windowsは dir /a かな？)
# .git というディレクトリが出来ているのが分かりますか？　これがリポジトリですよ

> git status
# 現在の状態を表示させます。内容的には「現在のブランチはmasterです。何も変更なしです」と出ています（ブランチについては後で説明します）

> vi newfile1.txt (Windowsは copy con かな？ メモ帳あたり使う？)
# 適当にファイルを新規作成して保存してください　ファイル名、内容は適当でいいです

> git status
# 現在の状態を表示させます。内容的には「新規ファイルがありますよ」と出ています

> git add newfile1.txt
# Gitにファイルを認識させます

> git status
# 現在の状態を表示させます。内容的には「このファイルを管理するけどいいかい？次のコミット対象だよ？」と出ています

> git commit -m "first commit"
# 現在の状態をコミットします。つまり、先ほどGitに認識させたファイルを正式に登録・管理状態にします

> git log
# 過去の歴史を表示します。「過去のコミットは1つ、"first commit" があるよ」と出ています。ログのサイズにもよりますが、終了するには Q を押す必要があるかもしれません

> git status
# 現在の状態を表示させます。内容的には「最後のコミットから、特に変更はないっす」と出ています

> vi newfile1.txt
# 適当にファイルを修正して保存してください　内容は適当でいいです

> vi newfile2.txt
# 適当にファイルを新規作成して保存してください　ファイル名、内容は適当でいいです

> git status
# 現在の状態を表示させます。内容的には「新規ファイル、あと修正されたファイルがあります」と出ています

> git diff
# 最後のコミットからの修正内容の確認ができます。上下キーで移動し、終了するには Q を押して下さい

> git add newfile1.txt newfile2.txt
# Gitに2つのファイルを認識させます

> git status
# 現在の状態を表示させます。内容的には「このファイルを管理するけどいいかい？次のコミット対象だよ？」と出ています

> git commit -m "second commit"
# 現在の状態をコミットします。つまり、先ほどGitに認識させたファイルを正式に登録・管理状態にします

> git log
# 過去の歴史を表示します。「過去のコミットは2つ、"first commit"、"second commit" があるよ」と出ています

> git status
# 現在の状態を表示させます。内容的には「最後のコミットから、特に変更はないっす」と出ています

> git checkout abcdef123456789
# ここには log で確認できる、first commit のズラズラ出る羅列（コミットID、コミットのハッシュ 等と呼びます）をコピペして下さい
# 最初に行ったコミット、first commit までソースが巻き戻りました

> cat newfile1.txt
# ファイルの中身を確認します。内容が最初の状態になっているのが分かりますか？

> git checkout master
# 最後に行ったコミットまでソースの状態を進めました
# この master というやつは "元に戻す" という意味では　ありません！　後で説明しますが、今回はまぁこれで元に戻るはずです

> cat newfile1.txt
# ファイルの中身を確認します。内容が修正後の状態になっているのが分かりますか？

> cd ../
> rm -fr git_test
# テスト用ディレクトリを削除しました。リポジトリ内に全ての歴史が保存されています
# リポジトリさえ残っていれば全てが復元できます。逆に言うとリポジトリごと消すだけで今テストで使用した全てが簡単に削除できます
```

こんな感じです。  
毎回、しつこいくらい git status で確認しています  
慣れないうちはとりあえず git status でいいと思います  

では基本的なコマンドの説明をはじめますよ

==========

####コマンド

**git add**  
「指定されたファイルをステージング領域へ登録するよ！」  

ステージング領域とは、この後で出てくるコミットの候補として登録する場所みたいなものです  
ステージング領域はインデックス領域、またはステージング、インデックス とも言う事もあります  
ぜんぶ同じような意味で覚えておいていいです  
「Gitの歴史に登録する準備をする」ような感じでいいです

```
> git add modified_file.txt
# カレントディレクトリの modified_file.txt をステージングします

> git add modified_file1.txt modified_file2.txt
# カレントディレクトリの modified_file1.txt と modified_file2.txt をステージングします

> git add app/modified_file.txt
# カレントディレクトリから見て相対パスにある、app/modified_file.txt をステージングします

> git add app/
# カレントディレクトリから見て相対パスにある、 app/ 配下の修正済みのファイルを全て追加します
# （修正済みのファイルと言いましたが、もちろん新規追加したファイルも含まれます）  
# 逆に、前回のコミットから何も変更されていない既存のファイルは add できない、ということですね

> git add .
# カレントディレクトリ配下のステージング可能なファイルを全て追加します
```

**git commit**  
「ステージングされたファイルをGitの歴史に記録するよ！たとえ君が忘れたとしても、永遠に」  

コミットする事で、歴史へ現在のファイルの状態を記録した事になります  
コミットにはメッセージを含める事が出来ます。どういった修正をコミットするのか、歴史にメモを残します  
メッセージは歴史の調査に必要になってきます。その時のために判りやすい説明を入力するべきです  

```
> git commit
# ステージング済みのファイルをコミットします。エディタが起動しますが慌てないで下さい。コミットメッセージを入力する画面です、普通に入力保存終了して下さい

> git commit -m "test"
# エディタを使用することなく、指定したメッセージでコミットします
```

**git log**  
「コミットの履歴を表示するよ！どういう歴史を経て現在の姿になったのか知りたいだろ？」  

コマンド名のとおり、ログを新しい順に表示します  
表示されるのは現在のブランチの歴史 となります。ブランチについては後述します  
コミットID、コミットした人の情報、日付、コミット時に指定したメッセージが確認できます  
コミットIDはこの後のコマンドで使用したります。逆に言うと、別のコマンドで使うコミットIDを調査する、とも言えます  
上下キー等で移動できます。「q」で元のプロンプトに戻ります。
```
> git log

commit be165cdab81e46e44e678cecb493d5d4db51d8b4
Author: mokoaki <mokoriso@gmail.com>
Date:   Wed Aug 5 17:15:17 2015 +0900

    AA対応

commit e0278ad0e64c708ae3c4437efcc95fc318ddadb5
Author: mokoaki <mokoriso@gmail.com>
Date:   Wed Aug 5 11:17:35 2015 +0900

    BBの修正
```

**git status**  
「君が修正、追加、削除、ステージングしたファイルを、僕がどのように認識しているかを表示するよ！」  

statusコマンドは現状の状態を確認できます。  
どのファイルがステージング済みであり、どのファイルが変更されたのか、新規追加されたファイルをGitがどのように認識しているかを表示させます
```
> git status

On branch master     ●「現在は master ブランチに居ます（後ほど説明します）」
Changes to be committed:     ●「コミット対象のファイル」（つまり、ステージング済み）」
  (use "git reset HEAD <file>..." to unstage)     ●「このコマンドでステージング解除できます」（後ほど説明します。resetコマンドです）」

  modified:   staged_file.txt     ●「修正済みファイル、staged_file.txt がステージング済みです」

Changes not staged for commit:     ●「修正されているが、ステージングされていないファイル」
  (use "git add <file>..." to update what will be committed)     ●「add コマンドでステージングできます」
  (use "git checkout -- <file>..." to discard changes in working directory)     ●「checkout コマンドで修正をなかったことにできます」（後ほど説明します）」

  modified:   modified_file.txt     ●「modified_file.txt が修正済みです」

Untracked files:     ●「管理されていないファイル（新規追加ファイル）」
  (use "git add <file>..." to include in what will be committed)     ●「add コマンドでステージングできます」（新規追加時にもadd）」

  new_file.txt     ●「new_file.txt があるみたいです」
```

**git diff**  
「君はこのファイルの、この部分を、このように修正してるらしいよ！」  

最後のコミットと、現在の作業内容のdiff（差分）を確認できます  
思わぬ修正をしていないか、等が簡単に調べられるのは心強いです  
ちなみにステージングしたファイルはdiffの対象にはなりません（厳密に言うと、ステージング領域と作業ファイルの差分を表示するためです。が、慣れるまでは最近修正した差分が見れる、程度に覚えておくといいと思います）  
そのあたりは設定で好きに変えられるので調べてみるといいかもしれません  
\- + が付いている行が修正された行であり、前後数行も一緒に表示されます。  
差分は行単位で表示されるので、行の中の一部分を修正していたとしても「-の行が削除されて、+の行が追加された」という感じに表示されます。このあたりは慣れが必要かもしれませんが、ぶっちゃけ慣れます
```
> git diff
diff --git a/mokotarou.txt b/mokotarou.txt
index a0815ba..9f59fe9 100644
--- a/mokotarou.txt
+++ b/mokotarou.txt
@@ -158,7 +158,9 @@ むかしむかし、おじいさんとおばあさんが
     すんでいました。
     おじいさんは山へ芝刈りに、
-    おばあさんは川へ選択に行きました
+    おばあさんは川へ洗濯に行きました
```

**git show**  
「コミット毎の詳細を見せてあげよう！」  

指定したコミットの詳細情報を表示します  
git show に続けてコミットIDを指定するとそのコミットの情報、  
省略した場合は最新の（最後に行った）コミットの情報を表示します。
git log と git diff の情報を合わせたようなものになります
```
>git show
commit be165cdab81e46e44e678cecb493d5d4db51d8b4
Author: mokoaki <mokoriso@gmail.com>
Date:   Wed Aug 5 17:15:17 2015 +0900

    AA対応

diff --git a/mokotarou.txt b/mokotarou.txt
index df859b3..a2cad8a 100644
--- a/mokotarou.txt
+++ b/mokotarou.txt
@@ -16,7 +16,7 @@ むかしむかし、おじいさんとおばあさんが
     すんでいました
     おじいさんは山へ芝刈りに
-    おばあさんは川へ選択に行きました
+    おばあさんは川へ洗濯に行きました
```

**git checkout**  
「その修正はちょっとダサいな。いったん無かった事にして、コミットしておいた時点からやりなおしてみないか？」  

checkout は大きく２つの機能がありますが、ここで説明するのは「修正内容をなかった事にする」です
修正を破棄し、最後のコミット時の内容にファイルを戻します  
```
> git status
> git checkout hoge.txt
> git status
```

#そろそろ、ややこしくなってきます
一人でバージョン管理するだけなら、これまでのコマンドでなんとかなるでしょう  
しかし、他人との連携、複数の作業を同時に行う、歴史の改変 等、もう少し手の込んだ作業を行おうと思ったら  
もう少し覚えないとダメそうなコマンドがまだあります  

その為にステージング領域あたりの説明をもう少し詳しくしましょう　そうしましょう  

----

![](https://raw.githubusercontent.com/mokoaki/git_no_are/master/images/setumei1.png)  
ワーキングコピー（作業領域）で修正したファイルを  
git add でステージング領域へ登録し  
git commit でリポジトリにコミットする  
までの流れのイメージです

![](https://raw.githubusercontent.com/mokoaki/git_no_are/master/images/setumei2.png)  
コミットを行い、ワーキングコピーからリポジトリまでが一致した状態からワーキングコピーのファイルを修正したイメージです  
git diff で差分が確認できるようになりました

![](https://raw.githubusercontent.com/mokoaki/git_no_are/master/images/setumei3.png)  
git add を行い、ステージング領域への登録をすましたイメージです  
ワーキングエリアとステージング領域の内容が一致しています

![](https://raw.githubusercontent.com/mokoaki/git_no_are/master/images/setumei4.png)  
git commit を行い、リポジトリへ新たなコミットを登録、歴史を進めたイメージです  
まさに、3つ前の状態と同じです。このようにして歴史が進んでいきます  
既に、次の作業が始まっているようです

**git reset**  
**git commit --amend**  
**git checkout（2回目）**  
**git branch**  
**git merge**  
**git rebase**  
**git push**  
**git fetch**  
**git pull**  
**git rebase -i**  
**git cherry-pick**  

####内部はどうなってる？
配管コマンド  

おまえの3週間のブランチ移動を無駄にしてはならない。なかったことにしてはいけない。  
いくつものブランチを旅してきたからこそ紅莉栖を助けたいと思うお前はそこにいる！Job-Hub開発に全てを捧げた俺がここにいる！  
おまえが立っているそのHEADは、俺達が紅莉栖を助けたいと願ったからこそ到達できたコミットIDなんだ！！

a  
a  
a  
a  
a  

addした後にコミットせずにファイル修正したらどうなる？  



###（後で説明・文章も大幅に修正しないといけない
####ブランチ（の説明に差し込む？）
判れば当然の事なのですが、99%のコマンドは現在のブランチに変更を加えます。
大事な事なのでもう一度言うと現在のブランチがコマンドの影響範囲です。
逆に言うと、現在以外のブランチには影響を及ぼしません。
ブランチを切る、ということは既存のソースの状態を保護する、という事でもあります。
コレを覚えておけば、例えば
```
# よし、hogehogeをマージするぞ！
git merge hogehoge
```
の時に、
マージされるのは何だ？　hogehogeがマージされるのか？
hogehogeからマージされるのか？でも何に？
と悩む必要がなくなります。
変更されるのは現在のブランチ（カレントブランチ）ですから、
hogehogeブランチをカレントブランチにマージする、その結果カレントブランチのHEADが伸びる
という事が判るかなと思います


###commit --amend
```
git commit --amend
```
ぐぐると前回のコミットを無かった事にしてコミットを上書きする、みたいな説明文がありますが、ウソです。  
現在のコミットの親コミットから新たにコミットを作成し、それをHEADにする、が正しいです。  
全ての過去、は無かったことにはなりません。ただ、参照するコミットが変わるだけです。

###コミットしないでブランチ移動
そこまで重要ではなく、「ああ、そういう動きするんだ」程度で  
できるパターンと出来ないパターン　移動元と移動後のブランチで、変更前のファイルが同じもの、であれば移動できる　説明むずい
