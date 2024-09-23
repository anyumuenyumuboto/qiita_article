---
title: VScodeで.gitディレクトリを表示する
tags:
  - Git
  - VSCode
private: false
updated_at: '2024-04-28T23:11:13+09:00'
id: e7cc0cc0cbe5f37d8ac6
organization_url_name: null
slide: false
ignorePublish: false
---
VScodeで.gitディレクトリの中身が見たいのに見つからない！
どうやら、デフォルトでは.gitは非表示になっているよう。
なので.gitが表示されるようにしてみた。

# 手順

「ファイル」→「ユーザー設定」→「設定」の順に選択

![スクリーンショット 2024-04-28 221349.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3780466/0c3d1614-9bf3-c784-2889-b40ad46ef12f.png)

「ユーザー」・「リモート（WSL:ubuntu）」・「ワークスペース」のどの範囲で有効な設定を編集するか選択。
左のナビゲーションウインドウを「テキストエディタ」→「ファイル」の順に選択
Excludeの**/.git の×をクリックして削除

![スクリーンショット 2024-04-28 221541.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3780466/f982dad2-f87a-85df-8897-e9cad079ff65.png)

以上！

--- 

簡単な設定ですが、windowsかつ日本語の環境での例が検索上位に見つからなかったので忘備録的に
初投稿なのでqiitaの投稿練習もかねて

# 参考

[Visual Studio Codeで.gitフォルダを表示する – Hubbit 開発者ブログ](https://techblog.hubbit.io/2020/10/05/visual-studio-code-show-git-folder/)

[VSCodeで.git配下のファイルを表示する - Nothing Behind](https://nothing-behind.com/2022/09/02/vscode%E3%81%A7-git%E9%85%8D%E4%B8%8B%E3%81%AE%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%82%92%E8%A1%A8%E7%A4%BA%E3%81%99%E3%82%8B/)
