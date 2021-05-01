# git_tutorial
## 確認したい操作一覧
- gitコマンドでは、--helpオプションをつけると、指定したコマンドの詳細情報を把握できる
- git switch
    - -cでブランチを新規作成して、checkoutする
- git restore
    - 指定したファイルなどの変更をなくす
- [git rebase](https://git-scm.com/book/ja/v2/Git-%E3%81%AE%E3%83%96%E3%83%A9%E3%83%B3%E3%83%81%E6%A9%9F%E8%83%BD-%E3%83%AA%E3%83%99%E3%83%BC%E3%82%B9)
    - --onto,
    - --continue: Restart the rebasing process after having resolved a merge conflict.,
    - --abort: Abort the rebase operation and reset HEAD to the original branch. If <branch> was provided when the rebase operation was started, then HEAD will be reset to <branch>. Otherwise HEAD will be reset to where it was when the rebase operation was started.,
    - -i: rebaseの対象となるcommtiのリストを作成し、rebaseする前にそれらを編集できるようにする
        - fixup: 指定したcommitメッセージを削除し、前のcommitと融合する
        - squash: 前のコミットと融合し、コミットメッセージを編集できる
        - edit: user commits, but stop for amending
            - この結果、指定したcommit時点に戻り、ファイルの編集などが可能になる。
            - そして、加えたい変更ができたらgit commit --amendすることで指定したcommitを新しいcommitに置き換える
            - そして、メッセージを編集する画面(vim)が出てくるので、メッセージを編集したければ編集する
            - その後、git rebase --continueで作業プロセスが進んで終了する
    - git pull --rebase
- git commit
    - インデックスの状態を記録する(addは、変更をインデックスに登録する)
    - --allow-empty
    - [--amend](https://book.git-scm.com/book/en/v2/Git-Basics-Undoing-Things): 新しいコミットを作成することで、現在のブランチの先端を置き換えます。=前のcommitの変更をindexに戻して、新しいコミットを作成できる。
    - ex) git commit(No1) → 前回のcommitに入れたかった編集をindexにステージングする。 → git commit --amendで、前のコミットの変更をindexに戻し、再度コミットする。
- git revert
    - rebert...元に戻す
    - 取り消したいこっ水戸を打ち消すようなコミットを新しく作成する
    - revertしたことがバージョン管理されるため、リモートにpushされて公開されているcommitに対しても安全に使えるらしい
    - メッセージの編集: -e or --edit(default)、編集しない: --no-edit
    - コミットしない: -n...indexに戻すだけでcommitまで行われないようにする
    - マージコミットの取り消しにおける、マージした2つのコミット(親)のうちどちらに戻すのかを指定する(オプションなしではできない): -m parent-number
    - ex) git revert HEAD~3(HEAD以降の3番目のcommitをrevert), git revert -n master~5..master~2
    - ex) git revert HEAD~4..HEAD(HEAD~4にはなくて、HEADには存在する全てのcommitをrevertする...git revert --continueするたびに、vimの編集画面が出てくる)
- [git tag](https://qiita.com/growsic/items/ed67e03fda5ab7ef9d08)
    - 特定のコミットに任意の名前をつけることができる=開発中のキリがいいコミット(リリースしたタイミングなど)に目印をつけておくことができる
    - 原則として、remoteにpushしたtagは削除・変更してはならない...tag更新しても、他の作業者のtagが自分の更新で上書きされないため...Gitはtagをバージョン管理していないgit
        - 参考文献: https://qiita.com/growsic/items/60928fc67c9efe373a73
        - 解決手法1. 別名のtagを利用する(健全なやり方)
        - 解決手法2. -fオプションで強制pushする。そして、そのリポジトリに関わる全ユーザに、古いtagを削除して、新しいtagをfetchしてもらう。(狂気的なやり方)
- git config
    - --gloabal or --local or --systemなどで、どこの設定ファイルを操作するかを決定できる
    - -lで一覧確認
    - user.nameやuser.emailは、commiterやauthorとして利用されるので、公開したくないメアドや名前が設定されていれば、変更すること
- [git rm](https://www.atmarkit.co.jp/ait/articles/2006/04/news022.html)
    - 指定したファイルをGitの管理対象から外し、ワークツリーやインデックスからも削除する
        - ワークツリー→インデックス→リポジトリ
    - ワークツリーに残しておきたい場合は、--cachedオプションを利用する
        - .gitignoreに記入し、gitの管理からはずしたかったのものを外すときに利用する
    - ディレクトリごと消したい場合は、-rオプションを利用する
- git cherry-pick
    - 指定したcommit(複数も可能)の変更を、取り込んで新しいcommitを生成する
    - コミットしたくない場合は、-nをつける
- git filter-brnach
    - そもそもそんなに使う場面がなさげ
    - 指定したブランチを、カスタムフィルターを適用して、gitの履歴を改訂する
        - 過去のcommiterやauthor全てを改変したい
        - 間違えてupしたパスワードファイルを完全に削除したい
    - オプション
        - --env-filter
            - commitが実行される環境を修正するだけの場合に使用する
            - author/committer/name/email/timeなどの環境変数
        - --index-filter
            - indexを書き換えるフィルタで、ツリーをチェックアウトしないので、tree-filterより高速
            - git rm --cached --ignore-unmatch...などが一緒によく使われている
        - etc
- git reset
    - git resetには3つのフォームがある
    1. git reset [-q] [<"tree-ish">] [--] <"paths">...
        -  git add <"paths">の逆
    2. git reset (--patch | -p) [<"tree-ish">] [--] [<"paths">...]
        - よくわからんので保留
    3. git reset [<"mode">] [<"commit">]: 一番使う
        - [参考文献](https://www.r-staffing.co.jp/engineer/entry/20191129_1)
        - 現在のブランチのヘッドを<"commit">にリセットして、<"mode">に応じて、インデックス(<"commit">のツリーにリセット)とワーキングツリーを更新できる。
        - <"mode">が省略された場合、defaultで--mixedになる
            - 主に利用されるものは、3つ(他にもある)
            - --soft: 指定したcommitからHEADまでのcommitが取り消されて、それらの変更内容がindexのステージ上に残る
            - --mixed: 指定したcommitからHEADまでのcommitが取り消されて、それらの変更内容がindexのステージ上からも下ろされて、ワーキングツリーに残る
            - --hard: 指定したcommitからHEADまでのcommitが取り消されて、それらの変更がindexのステージやワーキングツリーからも消える
- detached HEADの理解
- github actions
    - [参考文献1](https://docs.github.com/ja/actions/learn-github-actions/introduction-to-github-actions)
- ssh接続

## 参考文献
- https://git-scm.com/book/en/v2/Git-Tools-Revision-Selection#_commit_ranges
- https://book.git-scm.com/