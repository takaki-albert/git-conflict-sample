# Gitコンフリクト解消練習問題

コンフリクト解消の練習問題です。

## 状況説明

このリポジトリには2つの文章ファイルが存在しています。(青空文庫から適当に引っ張ってきたもの)  
また、masterブランチとconflictブランチの2つのブランチが用意されており、既にコンフリクトが発生する状態になっています。  

このリポジトリでconflictブランチからmasterブランチへマージするためのコンフリクト解消手順を練習できます。

## 準備

```
$ git clone https://github.com/takaki-albert/git-conflict-sample
$ git fetch
$ git branch conflict remotes/origin/conflict
```

## コンフリクトの解消方法

下記のどちらかの方法でコンフリクトの改修を行うことができます。  
Githubを利用している場合、基本的には1の方法で対応可能です。

1. GithubのPRページ上でコンフリクトを解消する
2. ローカルでgitコマンドなどを駆使してコンフリクトを解消する

## 1. GithubのPRページでコンフリクト解消をする手順

0. PRを作成する（今回の場合conflictブランチからmasterプランチに向けて）
0. PRページの `Resolve Conflicts` からコンフリクト修正ページへ繊維
0. [コンフリクトの直し方](#%e3%82%b3%e3%83%b3%e3%83%95%e3%83%aa%e3%82%af%e3%83%88%e3%81%ae%e7%9b%b4%e3%81%97%e6%96%b9)を参考に全てのコンフリクトを修正
    - ファイルごとに修正完了したら `resolve conflicts` ボタンが活性化されるので押す
    - 全てのファイルを `resolve conflicts` すると `commit` ボタンが現れるので押す
0. コンフリクトが解消されてマージをすることができるようになるのでマージして完了

## 2. gitコマンドを駆使してコンフリクト解消する手順

0. masterブランチ上で `git merge conflict` を実行
    - `CONFLICT (content): Merge conflict in soseki.txt` などと表示されてマージに失敗する
0. `git status` コマンドでコンフリクトしているファイルを確認
0.  [コンフリクトの直し方](#%e3%82%b3%e3%83%b3%e3%83%95%e3%83%aa%e3%82%af%e3%83%88%e3%81%ae%e7%9b%b4%e3%81%97%e6%96%b9) を参考に全てのコンフリクトを修正
0. コンフリクトしていたファイルをあらためて `git add` で取り込んで `git commit` する
0. `git push origin master` して完了

### コンフリクトの発生例

```
~/git-conflict-sample (master) $ git merge conflict
Auto-merging soseki.txt
CONFLICT (content): Merge conflict in soseki.txt
Auto-merging kenji.txt
CONFLICT (content): Merge conflict in kenji.txt
Automatic merge failed; fix conflicts and then commit the result.
```

### コンフリクトしたファイルの確認

git status を確認するとコンフリクトしたファイルが `both modified` と表示されます。

```
~/git-conflict-sample (master) $ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

        both modified:   kenji.txt
        both modified:   soseki.txt
```

## コンフリクトの直し方

コンフリクトしたファイルを確認すると、以下のようにコンフリクトの内容が表示されています。

```
<<<<<<< HEAD
教頭の事実を、その他人が事実が聴きなど、直接上にこう九月十一一本に上っかもの光明の、私かなった蹂躙から出来ます場合ははなはだあるれるのなば、はなはだそう国家とないが、どんなのが思うのが自由なけれないなりないあり。
=======
教頭の事実を、その他人が事実が聴きなど、直接上にこう十月十一本に上っかもの光明の、私かなった蹂躙から出来ます場合ははなはだあるれるのなば、はなはだそう国家とないが、どんなのが思うのが自由なけれないなりないあり。
>>>>>>> conflict
```

`======` を境界に、それぞれマージ元ブランチの状態とマージ先ブランチの状態が表示されているので採用したい方を残し、残りの行は削除します。
たとえば、conflictブランチの変更を採用したい場合は上記の5行から不用な行を削除して以下のように修正します。

```
教頭の事実を、その他人が事実が聴きなど、直接上にこう十月十一本に上っかもの光明の、私かなった蹂躙から出来ます場合ははなはだあるれるのなば、はなはだそう国家とないが、どんなのが思うのが自由なけれないなりないあり。
```

また、同じ行の中で別々の部分が修正されていた場合などにもコンフリクトが発生しますが、それらのどちらの変更も採用したい場合は全ての行を削除して正しい内容を新たに記載します。

```
<<<<<<< HEAD
及び変にはその頭の立派申がほかから思うないためから飽いてもっとも意味申すてい九月を済んものなく。

しかしそれはそのところよりし合うのん、担任の途が約束行っで仕方とは思っただって好いは亡びるないで。
=======
及び変にはその頭の立派申がほかから思うないためから飽いてもっとも意味申すてい十月を済んものなく。しかしそれはそのところよりし合うのん、担任の途が約束行っで仕方とは思っただって好いは亡びるないで。
>>>>>>> conflict
```

今回の例ではconflictブランチ側で10月に変更していて、masterがわで段落分割をしている状態。  
それらの変更をどちらも採用したい場合以下のとおりファイルを変更します。

```
及び変にはその頭の立派申がほかから思うないためから飽いてもっとも意味申すてい十月を済んものなく。

しかしそれはそのところよりし合うのん、担任の途が約束行っで仕方とは思っただって好いは亡びるないで。
```

同じように、kenji.txtも両方の変更が採用されるように修正してみてください。


## コンフリクトの解消を行っていたが、ぐちゃくちゃになって良く分からなくなってしまったら

Githubの場合、コンフリクト修正ページを閉じてしまえば作業内容が破棄されて最初からやり直すことができます。
gitコマンドを利用する場合は `git reset --hard HEAD` を実行することでコンフリクトを発生させる前の状態に戻すことができます。
