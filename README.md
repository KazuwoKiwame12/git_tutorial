# git_tutorial
## 確認したい操作一覧
参考文献: https://book.git-scm.com/
- git switch
    - -cでブランチを新規作成して、checkoutする
- git restore
    - 指定したファイルなどの変更をなくす
- [git rebase](https://git-scm.com/book/ja/v2/Git-%E3%81%AE%E3%83%96%E3%83%A9%E3%83%B3%E3%83%81%E6%A9%9F%E8%83%BD-%E3%83%AA%E3%83%99%E3%83%BC%E3%82%B9)
    - --onto,
    - --continue,
    - --abort,
    - -i
        - fixup: 指定したcommitメッセージを削除し、前のcommitと融合する
        - squash: 前のコミットと融合し、コミットメッセージを編集できる
    - git pull --rebase
- git commit
    - --allow-empty
    - [--amend](https://book.git-scm.com/book/en/v2/Git-Basics-Undoing-Things): 前のcommitの結果を置き換える
- git revert
- git tag
- git rm
- cherry-pick
- github actions
- ssh接続