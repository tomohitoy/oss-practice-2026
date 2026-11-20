# oss-practice-2026
拓殖大学工学部　2026年度『オープンソースソフトウェア』の実習用リポジトリです

## 講義内容

### [第5回：Gitの基本操作（add と commit）](./chap05/README.md)

Gitの基本的なコマンドである `git add` と `git commit` について学びます．
自己紹介ファイル `PROFILE.md` を書きながら，変更の記録方法や，コミットの仕組みを理解します．
ここで書いた `PROFILE.md` は，第6回で生成AIを使ってプロフィールサイトを作るときの素材になります．

### [第6回：Gitブランチ実習（生成AIでポートフォリオサイトを作り比べる）](./chap06/README.md)

Gitのブランチについて学びます．
第5回で書いた `PROFILE.md` をもとに，生成AIで2種類のデザインのサイトを別々のブランチで作り，切り替えて比べます．
ブランチの正体（`.git/refs/heads` と `.git/HEAD`）をのぞき，`git log --graph` と `git diff` で枝分かれと差分を確かめます．
作ったサイトは，第7回で `git push` して GitHub Pages（`https://ユーザー名.github.io/`）で公開します．

### [第7回：GitHubでサイトを公開する（git push と GitHub Pages）](./chap07/README.md)

GitHubアカウントとSSH鍵を用意し，ローカルとリモートの間で履歴をやり取りする仕組み（`git clone`・`git push`・`origin/main`）を学びます．
テンプレート（[tomohitoy/oss-template-2026](https://github.com/tomohitoy/oss-template-2026)）から `ユーザー名.github.io` リポジトリを作り，第6回で作ったサイトを `git push` して，`https://ユーザー名.github.io/` として公開します．

### [第8回：プルリクエストを送る（Fork と Pull Request）](./chap08/README.md)

書きこむ権限のない他人のリポジトリに，変更を提案する方法（Fork とプルリクエスト）を学びます．
受講生のサイトの一覧（[tomohitoy/oss-gallery-2026](https://github.com/tomohitoy/oss-gallery-2026)）を Fork し，ブランチを作って自分のファイルを追加し，「私のサイトも載せてください」というプルリクエストを送ります．
マージされると，[一覧ページ](https://tomohitoy.github.io/oss-gallery-2026/)に，第7回で公開した自分のサイトが並びます．

## テスト用環境の立ち上げ

何も設定されていない環境を `docker` を利用することで起動できます．

### docker container のビルド

```bash
docker build -t git-ubuntu .
```

### ubuntu イメージの起動

```bash
docker run -it --rm -v "$(pwd):/work" git-ubuntu
```

