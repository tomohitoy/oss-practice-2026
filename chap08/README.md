# 第8回：プルリクエストを送る（Fork と Pull Request）

## この回で学ぶこと
第7回では，**自分の**リポジトリに `git push` して，サイトを公開しました．
でも，世の中のオープンソースソフトウェア（OSS）のリポジトリは，ほとんどが**他人の**リポジトリです．書きこむ権限はありません．
それなのに，世界中の人がOSSの開発に参加できるのは，なぜでしょうか．

その答えが，この回で学ぶ **Fork（フォーク）** と **プルリクエスト（Pull Request）** です．

題材は，受講生のサイトを集めた「みんなのサイト」の一覧ページです．
教員のリポジトリに「私のサイトも一覧に載せてください」というプルリクエストを送り，取りこんでもらうことで，OSSに参加するときの流れをひととおり体験します．

### 今日のゴール
- [ ] 教員のリポジトリ（`tomohitoy/oss-gallery-2026`）を **Fork** する
- [ ] 自分の Fork を `git clone` し，名前とメールアドレスを設定する
- [ ] `add-my-site` ブランチを作り，`members/ユーザー名.md` を追加してコミットする
- [ ] `bash scripts/check.sh` で確認してから，ブランチを `git push` する
- [ ] 教員のリポジトリに向けて，**プルリクエスト**を作る
- [ ] 演習課題（プルリクエストのURL）を提出する

### 事前に用意するもの
- 第7回で公開したサイト（`https://ユーザー名.github.io/` をブラウザで開けること）
- GitHubアカウントと，第7回で登録したSSH鍵
- 第7回の1-Cでメモした，GitHubの noreply アドレス（`12345678+ユーザー名@users.noreply.github.com` の形のもの）

> noreply アドレスを忘れた人は，GitHubの右上の自分のアイコン → **Settings** → **Emails** を開くと，**Keep my email addresses private** の説明文の中に表示されています．

> この資料では，第7回と同じく，GitHubのユーザー名が `taku-shoku` の人を例にします．
> `ユーザー名` や `taku-shoku` と書いてあるところは，**自分のユーザー名に置きかえて**ください．

## Fork とプルリクエスト

### 書きこめないリポジトリに，どうやって参加する？
第7回で確認したとおり，他人のリポジトリに `git push` することはできません．
そこで，OSSでは次のように進めます．

1. 相手のリポジトリを，自分のアカウントにまるごとコピーする（**Fork**）
2. コピーした**自分の**リポジトリで，変更をコミットして push する（自分のものなので，push できる）
3. 相手に「この変更を取りこんでもらえませんか？」と提案する（**プルリクエスト**）
4. 相手が内容を確認して，よければ取りこむ（**マージ**）

**プルリクエスト（Pull Request）** は，「私の変更を pull して（取りこんで）ください」という request（お願い）です．
略して **PR**（ピーアール）や「プルリク」とも呼ばれます．この資料でも，ここから先は **PR** と書きます．

PRは，送ったら必ず取りこまれるわけではありません．
リポジトリを管理している人（**メンテナー**）が内容を確認し，「ここを直してください」とコメントを返すこともあります．
PRは，変更を送るだけの場所ではなく，**変更について相談する場所**でもあるのです．

### 3つのリポジトリ
今日は，同じ名前のリポジトリが3か所に登場します．

| 呼び名 | 場所 | 今日の例 | あなたは push できる？ |
| --- | --- | --- | --- |
| **upstream**（アップストリーム．大本） | GitHub | `tomohitoy/oss-gallery-2026` | できない |
| **origin**（オリジン．自分の Fork） | GitHub | `taku-shoku/oss-gallery-2026` | **できる** |
| **ローカル** | 自分のパソコン | `oss-gallery-2026` フォルダ | （自分のパソコンの中） |

### 「Use this template」と「Fork」のちがい
第7回の **Use this template** も，教員のリポジトリを自分のアカウントにコピーする操作でした．
Fork とは，何がちがうのでしょうか．

| | Use this template（第7回） | Fork（今日） |
| --- | --- | --- |
| 目的 | ひな形をもとに，**自分の新しいもの**を作る | **元のリポジトリに，変更を提案する** |
| コミットの履歴 | 引きつがない（`Initial commit` から始まる） | 元の履歴を，そのまま引きつぐ |
| 元のリポジトリとのつながり | ない | ある（`forked from ...` と表示される） |
| 元のリポジトリにPRを送れる？ | 送れない | **送れる** |

### 今日の全体像
```
 GitHub   tomohitoy/oss-gallery-2026          <- 教員のリポジトリ（upstream）．push できない
            |                   ^
            | (1) Fork          | (5) プルリクエスト（「取りこんでください」と提案）
            v                   |
 GitHub   taku-shoku/oss-gallery-2026         <- あなたの Fork（origin）．push できる
            |                   ^
            | (2) clone         | (4) git push origin add-my-site
            v                   |
 PC       oss-gallery-2026/                   <- あなたのパソコンのフォルダ
            (3) add-my-site ブランチを作って，members/taku-shoku.md を追加して commit

 (6) 教員が確認してマージすると，一覧ページに載る --> https://tomohitoy.github.io/oss-gallery-2026/
```

## 実習の流れ
1. 実習1：教員のリポジトリを Fork する
2. 実習2：Fork を clone して，準備する
3. 実習3：ブランチを作って，自分のファイルを追加する
4. 実習4：セルフチェックをして，push する
5. 実習5：プルリクエストを作る
6. 演習課題：プルリクエストのURLを提出する

それでは，順番に進めていきましょう．

---

## 実習1：教員のリポジトリを Fork する

### 1-A. 一覧のリポジトリとページを見る
ブラウザで，教員のリポジトリを開きます．GitHubにログインしていることも確認しましょう．

```
https://github.com/tomohitoy/oss-gallery-2026
```

`members` フォルダを開いてみましょう．ここに，1人につき1つのファイル（`ユーザー名.md`）が並びます．
このファイルをもとに，次の一覧ページが自動で作られています．

```
https://tomohitoy.github.io/oss-gallery-2026/
```

今日のゴールは，**`members` フォルダに自分のファイルを追加してもらって，この一覧ページに自分のサイトを載せること**です．

### 1-B. Fork する
1. 教員のリポジトリのページの右上にある **Fork** ボタンをクリックします

> **Fork ボタンが見つからない／押せないとき**
> GitHubにログインしているか確認しましょう．次のURLを開いても，同じ作成画面に進めます．
>
> ```
> https://github.com/tomohitoy/oss-gallery-2026/fork
> ```

2. 「Create a new fork」という画面で，次のようになっているか確認します

| 項目 | 設定 |
| --- | --- |
| **Owner** | 自分のユーザー名を選ぶ |
| **Repository name** | `oss-gallery-2026` のまま（変えない） |
| **Description** | そのままでOK |
| **Copy the `main` branch only** | チェックが入ったまま |

3. **Create fork** をクリックします

`taku-shoku/oss-gallery-2026` のようなページが表示され，リポジトリ名の下に `forked from tomohitoy/oss-gallery-2026` と表示されれば成功です．

**ポイント**：見た目は教員のリポジトリとそっくりですが，左上の名前が**自分のユーザー名**で始まっています．
これが，あなたの Fork（origin）です．第7回の `generated from ...` とのちがいにも注目しましょう．

---

## 実習2：Fork を clone して，準備する

### 2-A. SSHの接続を確かめる
ターミナル（Windowsは **Git Bash**）を開いて，第7回と同じコマンドで接続を確かめます．

```bash
ssh -T git@github.com
```

次のように，自分のユーザー名が表示されればOKです．

```
Hi taku-shoku! You've successfully authenticated, but GitHub does not provide shell access.
```

### 2-B. 自分の Fork を clone する

#### 1. SSHのURLをコピーする
**自分の Fork**（`ユーザー名/oss-gallery-2026`）のページで，次の順に操作します．

1. 緑色の **Code** ボタンをクリックします
2. **Local** タブの中の **SSH** を選びます（**HTTPS ではありません**）
3. `git@github.com:` で始まるURLをコピーします

```
git@github.com:taku-shoku/oss-gallery-2026.git
```

**重要**：URLが `git@github.com:tomohitoy/...` になっていたら，教員のリポジトリを開いています．**自分のユーザー名**になっていることを確認してください．

#### 2. clone する場所に移動する
第7回の `ユーザー名.github.io` フォルダと，同じ場所に clone します．
ホームフォルダに置いた人は，次のコマンドで移動できます．

```bash
cd ~
```
```bash
ls
```

`oss-practice-2026` や `ユーザー名.github.io` が表示されればOKです．

#### 3. clone する
`git clone` のあとに，コピーしたURLを貼り付けて実行します．

```bash
git clone git@github.com:ユーザー名/oss-gallery-2026.git
```

これで，フォルダがこのように並びました．

```
ホームフォルダ など
 |
 +-- oss-practice-2026/       <- 授業の資料（第5回）
 |
 +-- taku-shoku.github.io/    <- 自分のサイトのリポジトリ（第7回）
 |
 +-- oss-gallery-2026/        <- 今日 clone した，自分の Fork
```

#### 4. clone したフォルダに移動して，リモートを確認する
```bash
cd oss-gallery-2026
```
```bash
git remote -v
```

表示例：
```
origin	git@github.com:taku-shoku/oss-gallery-2026.git (fetch)
origin	git@github.com:taku-shoku/oss-gallery-2026.git (push)
```

`origin` が**自分のユーザー名**のリポジトリになっていることを確認します．
`tomohitoy` になっていた人は，教員のリポジトリを clone してしまっています（「困ったときは」を参照）．

### 2-C. コミットに記録される名前とメールアドレスを設定する（重要）
第7回の4-Cでは，`--global` をつけずに，`ユーザー名.github.io` リポジトリ**だけ**に公開用の名前とメールアドレスを設定しました．
`oss-gallery-2026` は別のリポジトリなので，**その設定は効いていません**．確認してみましょう．

```bash
git config user.name
```
```bash
git config user.email
```

本名のフルネームや，大学・個人のメールアドレスが表示された人は，第7回と同じように，**このリポジトリにも**公開用の名前とアドレスを設定します．

```bash
git config user.name "taku-shoku"
```
```bash
git config user.email "12345678+taku-shoku@users.noreply.github.com"
```

`taku-shoku` は自分のユーザー名に，メールアドレスは自分の noreply アドレスに置きかえてください．

> **なぜ，今日は特に大切なの？**
> PRがマージされると，あなたのコミットは，**そのまま教員のリポジトリの履歴に入ります**．
> 記録された名前とメールアドレスは，全世界から見えるうえに，あなたが自分で消すことはできません．

---

## 実習3：ブランチを作って，自分のファイルを追加する

### 3-A. PR用のブランチを作る
第6回で学んだ `git switch -c` で，ブランチを作って移動します．

```bash
git switch -c add-my-site
```
```bash
git branch
```

表示例：
```
* add-my-site
  main
```

`* add-my-site` と表示されればOKです．

**なぜ，ブランチを作るの？**
PRは，「**このブランチ**の変更を，**あのブランチ**に取りこんでください」という提案だからです．
`main` は大本（upstream）と同じ状態のまま残しておき，提案したい変更だけを別のブランチにまとめます．
ブランチの名前は，何をするためのものかがわかる名前にします（今日は，全員 `add-my-site` にそろえます）．

### 3-B. members フォルダを見る
```bash
ls members
```

表示例：
```
README.md     _template.md
```

> すでにマージされた人がいる場合は，その人たちのファイルも並んでいます．

`_template.md` が，自分のファイルを作るためのひな形です．中身を見てみましょう．

```bash
cat members/_template.md
```

```markdown
- **ニックネーム**：
- **サイトのURL**：https://ユーザー名.github.io/
- **ひとこと**：
```

### 3-C. ひな形をコピーして，自分のファイルを作る
ひな形をコピーして，**自分のユーザー名のファイル**を作ります（`taku-shoku` は自分のユーザー名に置きかえます）．

```bash
cp members/_template.md members/taku-shoku.md
```

**重要**：ファイル名は，**GitHubのユーザー名と同じ**にします．
一覧ページのリンク先（`https://ユーザー名.github.io/`）は，このファイル名から作られます．

エディタで `members/taku-shoku.md` を開き，「：」のあとを書いて保存します．

```markdown
- **ニックネーム**：たく
- **サイトのURL**：https://taku-shoku.github.io/
- **ひとこと**：週末はだいたい自転車に乗っています．好きな色の緑でまとめました．
```

| 項目 | 書くこと |
| --- | --- |
| **ニックネーム** | 一覧に大きく表示される名前．**本名のフルネームは書かないでください** |
| **サイトのURL** | `https://自分のユーザー名.github.io/`（「ユーザー名」を書きかえます） |
| **ひとこと** | サイトの紹介や見てほしいところを，1〜2文で（100文字以内が目安） |

**書くときの注意**：
- `- **ニックネーム**：` のような行の形は，変えないでください（「：」のあとに書きます）
- `_template.md` そのものは，書きかえません（書きかえるのは，コピーして作った自分のファイルだけです）
- このファイルは**全世界に公開されます**．学籍番号やメールアドレスなど，公開したくない情報は書かないでください

書けたら，中身を確認します．

```bash
cat members/taku-shoku.md
```

### 3-D. コミットする
```bash
git status
```

表示例：
```
On branch add-my-site
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	members/taku-shoku.md

nothing added to commit but untracked files present (use "git add" to track)
```

表示されているのが，**自分のファイル1つだけ**であることを確認します．
ほかのファイルが表示された人は，「困ったときは」を見て元に戻しましょう．

```bash
git add members/taku-shoku.md
```
```bash
git commit -m "feat: taku-shoku のサイトを一覧に追加する"
```

履歴を見てみましょう．

```bash
git log --oneline -3
```

表示例：
```
895fa04 (HEAD -> add-my-site) feat: taku-shoku のサイトを一覧に追加する
5b0575f (origin/main, origin/HEAD, main) Initial commit
```

> 2行目より下に表示されるコミットは，clone したタイミングによって変わります．

**見方**：
- `add-my-site` だけが，いま作ったコミットに進みました
- `main` と `origin/main` は，Fork したときのまま動いていません

第6回で学んだとおり，ブランチは「コミットを指すポインタ」でした．
この**「`main` から `add-my-site` までの差」が，これから提案する変更**になります．

---

## 実習4：セルフチェックをして，push する

### 4-A. セルフチェックをする
第7回と同じように，このリポジトリにもチェック用のスクリプトが入っています．

```bash
bash scripts/check.sh
```

このスクリプトは，ファイルやGitの設定を何も書きかえず，次のことを確認して表示するだけです．
- `origin` が自分の Fork になっているか，PR用のブランチにいるか
- `members/自分のユーザー名.md` があって，3つの項目が書かれているか
- 自分のファイルのほかに，変更してしまったファイルがないか
- コミットに記録された名前とメールアドレス（2-Cの設定）

表示例（後半）：
```
== 3. 変更したファイル（プルリクエストに入るもの） ==
  [OK]   変更したのは members/taku-shoku.md の1ファイルだけです

== 4. コミットに記録される名前とメールアドレス（全世界から見えます） ==
         名前：taku-shoku
         メール：12345678+taku-shoku@users.noreply.github.com
  [OK]   名前が設定されています（公開してよい名前か，上の表示を確認しましょう）
  [OK]   メールアドレスは GitHub の noreply アドレスです

== 5. Git の状態 ==
  [注意] まだ push していないコミットがあります
         -> git push origin add-my-site で GitHub に送りましょう

== 結果 ==
  [NG] はありません．[注意] の 1 個を確認してから進みましょう
  ※ このチェックは目安です．最後は，GitHubの「Files changed」で自分の目で確認しましょう
```

- **[NG]**：push する前に，必ず直してください．`->` のあとにヒントが表示されます
- **[注意]**：問題ないか，自分の目で確認してください．この時点では「まだ push していないコミットがあります」と出るのが正常です

ファイルを直した場合は，もう一度 `git add` → `git commit` してから，`bash scripts/check.sh` で確認しましょう（コミットは，いくつ重ねてもかまいません）．

> 2-Cの設定をする前にコミットしてしまった人は，「自分が作ったコミットに，次のアドレスが記録されています」と表示されます．
> 2-Cの設定をしてから，次のコマンドで直前のコミットを作り直しましょう．
> ```bash
> git commit --amend --reset-author --no-edit
> ```
> このコマンドで作り直せるのは，**直前の1つ**のコミットだけです．
> コミットを2つ以上作っていた人には，`bash scripts/check.sh` が別のコマンドを表示するので，そちらを実行してください．

### 4-B. ブランチを push する
`add-my-site` ブランチを，GitHubの自分の Fork に送ります．

```bash
git push origin add-my-site
```

**コマンドの意味**：
- `origin`：送り先のリモートリポジトリ．今日の `origin` は，**自分の Fork** です
- `add-my-site`：送るブランチの名前．第7回は `main` でしたが，今日は**作ったブランチ**を送ります

表示例：
```
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 8 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (4/4), 478 bytes | 478.00 KiB/s, done.
Total 4 (delta 1), reused 0 (delta 0), pack-reused 0
remote:
remote: Create a pull request for 'add-my-site' on GitHub by visiting:
remote:      https://github.com/taku-shoku/oss-gallery-2026/pull/new/add-my-site
remote:
To github.com:taku-shoku/oss-gallery-2026.git
 * [new branch]      add-my-site -> add-my-site
```

- `* [new branch]`：GitHubの自分の Fork に，`add-my-site` という新しいブランチができました
- `remote:` で始まる行は，GitHubからのメッセージです．「このブランチでPRを作れますよ」と教えてくれています

もう一度，履歴を見てみましょう．

```bash
git log --oneline -3
```

表示例：
```
895fa04 (HEAD -> add-my-site, origin/add-my-site) feat: taku-shoku のサイトを一覧に追加する
5b0575f (origin/main, origin/HEAD, main) Initial commit
```

`origin/add-my-site` が増えました．あなたのコミットが，GitHubの自分の Fork に届いた証拠です．

**ポイント**：この時点では，まだ**自分の Fork の中**にコミットがあるだけです．教員のリポジトリには，何も起きていません．

---

## 実習5：プルリクエストを作る
ここからは，ブラウザで操作します．

### 5-A. PRの作成画面を開く
ブラウザで，**自分の Fork**（`https://github.com/ユーザー名/oss-gallery-2026`）を再読み込みします．
ファイルの一覧の上に，次のような黄色い帯が表示されます．

```
add-my-site had recent pushes 1 minute ago        [ Compare & pull request ]
```

緑色の **Compare & pull request** ボタンをクリックします．

> **黄色い帯が表示されないとき**
> 次のURLの `ユーザー名` を自分のユーザー名に書きかえて，ブラウザで開きましょう．同じ作成画面に進めます．
>
> ```
> https://github.com/tomohitoy/oss-gallery-2026/compare/main...ユーザー名:add-my-site?expand=1
> ```

### 5-B. PRの向きを確認する（重要）
「Open a pull request」という画面の上のほうに，次のような表示があります．

```
base repository: tomohitoy/oss-gallery-2026   base: main   <-   head repository: taku-shoku/oss-gallery-2026   compare: add-my-site
```

矢印の向きが，**変更を取りこむ向き**です．次のようになっているか，必ず確認してください．

| 項目 | 意味 | 設定 |
| --- | --- | --- |
| **base repository** | 取りこんでもらう先のリポジトリ | `tomohitoy/oss-gallery-2026` |
| **base** | 取りこんでもらう先のブランチ | `main` |
| **head repository** | 変更があるリポジトリ（自分の Fork） | `ユーザー名/oss-gallery-2026` |
| **compare** | 変更があるブランチ | `add-my-site` |

かんたんに言うと，**`tomohitoy` の `main` ← `自分` の `add-my-site`** になっていればOKです．
そのすぐ下に，緑色で **Able to merge.** と表示されていることも確認します．

> ちがっている場合は，それぞれのメニューをクリックして選び直します．
> 特に，**base repository が自分のユーザー名になっていないか**注意してください（自分の Fork に向けてPRを送っても，教員には届きません）．

### 5-C. タイトルと本文を書く
PRには，**タイトル**と**本文**があります．メンテナーが最初に読むところです．

#### タイトル（Add a title）
コミットが1つだけのときは，**コミットメッセージが自動で入っています**．

```
feat: taku-shoku のサイトを一覧に追加する
```

何をするPRなのかが，ひと目でわかるタイトルになっていればOKです．空になっていたら，上の例にならって書きましょう．

#### 本文（Add a description）
本文の欄には，このリポジトリが用意した**ひな形**（PRテンプレート）が自動で入っています．

```markdown
## 概要
<!-- 何をするプルリクエストかを，1文で書きましょう．例：taku-shoku のサイトを，一覧に追加します． -->


## 追加するサイト
<!-- 下のURLの「ユーザー名」を，自分のユーザー名に書きかえましょう -->
https://ユーザー名.github.io/

## 確認したこと
<!-- 確認できたものは，[ ] を [x] に書きかえましょう（プルリクエストを作ったあとなら，クリックでもチェックできます） -->
- [ ] 追加したのは `members/自分のユーザー名.md` の1ファイルだけです
- [ ] `bash scripts/check.sh` で，[NG] が出ないことを確認しました
- [ ] 上のURLをブラウザで開いて，自分のサイトが表示されることを確認しました
- [ ] 本名のフルネーム・学籍番号など，公開したくない情報は書いていません

## ひとこと（任意）
<!-- サイトで工夫したところ，見てほしいところ，自信がないところなどを，自由に書きましょう -->

```

> ひな形が入っていない場合は，上の内容をコピーして，本文の欄に貼り付けましょう．
> `<!--` から `-->` までは，画面には表示されないメモ書きです．消しても，残してもかまいません．

次のように書きます．

1. **概要**：何をするPRなのかを，1文で書きます
2. **追加するサイト**：URLの「ユーザー名」を，自分のユーザー名に書きかえます
3. **確認したこと**：実際に確認できたものは，`[ ]` を `[x]` に書きかえます（`x` は半角の小文字．かっこの中にスペースは入れません）
4. **ひとこと（任意）**：サイトで工夫したところ，見てほしいところ，自信がないところなどを書きます（**書くと加点**されます）

記入例：
```markdown
## 概要
taku-shoku のサイトを，一覧に追加します．

## 追加するサイト
https://taku-shoku.github.io/

## 確認したこと
- [x] 追加したのは `members/自分のユーザー名.md` の1ファイルだけです
- [x] `bash scripts/check.sh` で，[NG] が出ないことを確認しました
- [x] 上のURLをブラウザで開いて，自分のサイトが表示されることを確認しました
- [x] 本名のフルネーム・学籍番号など，公開したくない情報は書いていません

## ひとこと（任意）
第6回で作った2つのデザインのうち，色づかいが気に入っているデザインAを公開しています．
スマートフォンで見たときに，見出しの文字が少し大きすぎるかもしれません．気になる点があれば教えてください．
よろしくお願いします．
```

本文の欄の上にある **Preview** タブをクリックすると，実際の表示を確認できます．

**ポイント**：
- PRの本文は，「何を」「なぜ」変更したのかを，**はじめて読む人**に伝えるための文章です
- OSSでは，顔を合わせたことのない人どうしが，文章だけでやり取りします．ていねいな言葉で書きましょう
- **PRのタイトルと本文も，全世界から見えます**．公開したくない情報は書かないでください

### 5-D. 送る変更を確認する
画面を下にスクロールすると，このPRに入る変更が表示されています．

- コミットが，自分の作ったものだけになっている
- 変更されたファイルが，`members/自分のユーザー名.md` の**1つだけ**で，緑色（追加）で表示されている

ほかのファイルが入っている場合は，PRを作らずに，「困ったときは」を見て直しましょう．

### 5-E. PRを作る
すべて確認できたら，緑色の **Create pull request** ボタンをクリックします．

> ボタンが **Draft pull request** になっている場合は，ボタンの右の ▼ から **Create pull request** を選び直してください．

教員のリポジトリのページに移動し，次のように表示されれば，**PRの作成は成功**です！

- タイトルの横に，`#12` のような番号がついている
- 緑色の **Open** のバッジが表示されている
- `taku-shoku wants to merge 1 commit into tomohitoy:main from taku-shoku:add-my-site` のように表示されている

**おめでとうございます．あなたの提案が，教員のリポジトリに届きました．**

ブラウザのアドレスバーのURLを見てみましょう．

```
https://github.com/tomohitoy/oss-gallery-2026/pull/12
```

URLが `tomohitoy/oss-gallery-2026` になっています．
PRは，あなたの Fork ではなく，**取りこんでもらう先（教員のリポジトリ）に作られる**のです．このURLが，演習課題の提出物になります．

PRのページには，3つのタブがあります．クリックして見てみましょう．

| タブ | 表示されるもの |
| --- | --- |
| **Conversation** | 本文と，コメントのやり取り |
| **Commits** | このPRに入っているコミットの一覧 |
| **Files changed** | 変更されたファイルと，その中身（差分） |

---

## 演習課題：プルリクエストのURLを提出する
作成したPRのURLを，課題に貼り付けて提出してください．

```
https://github.com/tomohitoy/oss-gallery-2026/pull/番号
```

### 提出前のチェック
- [ ] URLが `https://github.com/tomohitoy/oss-gallery-2026/pull/番号` の形になっている（`tomohitoy` の部分が，自分のユーザー名になっていない）
- [ ] **Files changed** タブに表示されるファイルが，`members/自分のユーザー名.md` の1つだけになっている
- [ ] タイトルを読めば，何をするPRなのかがわかる
- [ ] 本文の「概要」と「追加するサイト」が書かれていて，「確認したこと」にチェックが入っている
- [ ] （加点）本文の「ひとこと」に，工夫したところ・見てほしいところ・自信がないところなどを書いている
- [ ] （加点）「発展」の「クラスメイトのPRにコメントする」をした（コメントしたPRのURLも，あわせて提出してください）

> PRが**マージされているかどうかは，提出の条件ではありません**．PRを作ることができたら，提出してください．

---

## プルリクエストを送ったあと

### マージされるのを待つ
教員がPRの内容を確認して，問題がなければマージします．
マージされると，PRのバッジが緑色の **Open** から，紫色の **Merged** に変わります（GitHubに登録したメールアドレスにも，お知らせが届きます）．

マージの数分後に，一覧ページを再読み込みしてみましょう．

```
https://tomohitoy.github.io/oss-gallery-2026/
```

**あなたのサイトが，一覧に並んでいるはずです．**
あなたの書いたファイルが，他人のリポジトリに取りこまれ，その成果物の一部になりました．これが，OSSに「貢献する（コントリビュートする）」ということです．

### 修正を頼まれたら（PRは作り直さない）
PRに「ここを直してください」というコメントがつくことがあります．
そのときは，PRを作り直すのではなく，**同じブランチにコミットを追加して push します**．

```bash
git switch add-my-site
```

エディタで自分のファイルを直して保存したら，コミットして push します．

```bash
git add members/taku-shoku.md
```
```bash
git commit -m "fix: ひとことの誤字を直す"
```
```bash
git push origin add-my-site
```

PRのページを再読み込みすると，**追加したコミットが，自動でPRに入っています**．

**なぜ，自動で入るの？**
PRが見ているのは，「コミット」ではなく「**ブランチ**」だからです．
ブランチはコミットを指すポインタで，`git push` は，GitHub側のポインタを進めるコマンドでした（第7回）．
`add-my-site` が新しいコミットを指すようになったので，それを見ているPRにも，新しいコミットが入ったのです．

直したら，PRの **Conversation** タブのいちばん下のコメント欄で，直したことを伝えましょう．

```markdown
ご指摘ありがとうございます．ひとことの誤字を直しました．
お手数ですが，もう一度ご確認をお願いします．
```

---

## 困ったときは

| 症状 | 原因と対処 |
| --- | --- |
| **Fork** ボタンを押すと，自分の Fork のページに移動する／`No available destinations to fork this repository.` と表示される | すでに Fork 済みです．`https://github.com/ユーザー名/oss-gallery-2026` を開いて，実習2に進みましょう |
| `git clone` で `Permission denied (publickey).` と表示される | SSH鍵の設定ができていません．2-Aの `ssh -T git@github.com` が成功するか確認しましょう（第7回の実習2を参照） |
| `git clone` で `Username for 'https://github.com':` と聞かれる | HTTPS のURLをコピーしています．`Ctrl + C` で中断し，2-Bの1で **SSH** のURLをコピーし直しましょう |
| `git remote -v` の `origin` が `tomohitoy` になっている／`git push` で `ERROR: Permission to tomohitoy/oss-gallery-2026.git denied to ユーザー名.` と表示される | 教員のリポジトリを clone しています．実習1の Fork ができていることを確認してから，`git remote set-url origin git@github.com:ユーザー名/oss-gallery-2026.git` で，`origin` を自分の Fork に変えましょう |
| `main` ブランチのまま，ファイルを作ったりコミットしたりしてしまった | そのまま `git switch -c add-my-site` を実行すれば，変更やコミットを持ったまま，新しいブランチに移れます |
| `fatal: a branch named 'add-my-site' already exists` と表示される | ブランチは作成済みです．`git switch add-my-site`（`-c` なし）で移動しましょう |
| ファイル名をまちがえた（`ユーザー名.md` のまま，など） | まだコミットしていないなら `mv members/まちがえた名前.md members/自分のユーザー名.md`，コミットしたあとなら `git mv members/まちがえた名前.md members/自分のユーザー名.md` → `git commit -m "fix: ファイル名を直す"` で直せます |
| `bash scripts/check.sh` で「自分のファイルのほかにも，変更されているファイルがあります」と表示される | 表示されたファイルを元に戻します．まちがえて書きかえたファイル（`members/_template.md` など）は `git restore --source origin/main ファイル名`，まちがえて作ったファイルは `rm ファイル名` で元に戻せます．すでにコミットしていた場合は，そのあとで `git add -A` → `git commit -m "fix: 関係のないファイルを元に戻す"` を実行しましょう |
| `bash scripts/check.sh` で `No such file or directory` と表示される | `oss-gallery-2026` のいちばん上のフォルダで実行していません．`pwd` で確認しましょう |
| `bash scripts/check.sh` で `$'\r': command not found` と表示される | エディタで開いたときに，改行コードが変わってしまっています．`rm scripts/check.sh` → `git restore scripts/check.sh` で元に戻しましょう |
| `git push` で `error: src refspec add-my-site does not match any` と表示される | `add-my-site` ブランチがないか，まだコミットがありません．`git branch` と `git log --oneline -3` で確認しましょう |
| `git push` で `GH007: Your push would publish a private email address.` と表示される | コミットに，非公開にしたメールアドレスが記録されています．2-Cの設定をしてから，`bash scripts/check.sh` の「4.」に表示されるコマンドでコミットを作り直して，もう一度 push しましょう |
| `git push` で `! [rejected]  add-my-site -> add-my-site (non-fast-forward)` と表示される | push したあとで，コミットを作り直しています（`--amend` など）．`git push --force-with-lease origin add-my-site` で，GitHubのブランチを，作り直したコミットに置きかえましょう．**このコマンドを使ってよいのは，自分しか使っていないブランチだけです** |
| PRの作成画面に `There isn't anything to compare.` と表示される | ①まだ push していません．`git push origin add-my-site` を実行しましょう ②**compare** が `main` になっています．`add-my-site` を選び直しましょう |
| PRのURLが `https://github.com/自分のユーザー名/...` になっている | **base repository** を自分の Fork にしてPRを作ってしまいました．そのPRのページのいちばん下にある **Close pull request** で閉じてから，5-Aの「黄色い帯が表示されないとき」のURLで作り直しましょう |
| PRを2つ作ってしまった | 片方のPRのページのいちばん下にある **Close pull request** で閉じましょう．提出するのは，残したほうのURLです |
| まちがえて **Close pull request** を押してしまった | 同じ場所にある **Reopen pull request** を押すと，元に戻せます |
| PRを作ったあとで，本文やタイトルを直したい | 本文は右上の **…** → **Edit**，タイトルはタイトルの右の **Edit** で直せます（PRを作り直す必要はありません） |
| マージされたのに，一覧ページに自分のサイトが出てこない | 反映には数分かかります．少し待ってから，強制再読み込み（Windows：`Ctrl + F5`，Mac：`Command + Shift + R`）をしてみましょう |
| 公開してはいけない情報を，ファイルやPRに書いてしまった | すぐに教員に相談してください |

## まとめ

### この回で学んだコマンド
今日は，新しいGitのコマンドはほとんど出てきませんでした．
**これまでに学んだコマンドの組み合わせだけで，他人のリポジトリに変更を提案できる**のです．

- **`git clone git@github.com:ユーザー名/リポジトリ名.git`**：自分の Fork をクローンする（第7回）
- **`git config user.email "アドレス"`**：いまいるリポジトリだけの設定をする（第7回）
- **`git switch -c ブランチ名`**：ブランチを作って移動する（第6回）
- **`git add`** → **`git commit -m "メッセージ"`**：変更を記録する（第5回）
- **`git push origin ブランチ名`**：`main` 以外のブランチを，リモート（`origin`）に送る
- **`bash scripts/check.sh`**：このリポジトリに入っている，PRを送る前のセルフチェック

### Fork とプルリクエスト
1. 他人のリポジトリには push できない．だから，**Fork** で自分のアカウントにコピーを作る
2. 大本のリポジトリを **upstream**，自分の Fork を **origin** と呼ぶ
3. 変更は `main` ではなく，**PR用のブランチ**にまとめて，自分の Fork に push する
4. **プルリクエスト（PR）** は，「このブランチの変更を取りこんでください」という提案．**向き（base ← head）** を必ず確認する
5. PRは，取りこんでもらう先（upstream）に作られる．タイトルと本文で，「何を」「なぜ」変更したのかを伝える
6. PRは**ブランチ**を見ている．同じブランチにコミットを追加して push すれば，PRは自動で更新される（作り直さない）

### OSS開発の流れ
これで，OSSに参加するときの基本の流れを，ひととおり体験しました．

1. **Fork**：大本のリポジトリを，自分のアカウントにコピーする
2. **Clone**：自分の Fork を，パソコンにコピーする
3. **Branch & Commit**：ブランチを作って，変更をコミットする
4. **Push**：ブランチを，自分の Fork に送る
5. **Pull Request**：大本のリポジトリに，変更を提案する
6. **Review**：メンテナーが内容を確認する．必要なら，コメントをもとに直す
7. **Merge**：大本のリポジトリに取りこまれる

世界中のOSSで，毎日この流れがくり返されています．

---

## 発展：もっと学びたい人へ

### クラスメイトのPRにコメントする（加点）
OSSでは，メンテナーだけでなく，**誰でも**ほかの人のPRを見て，コメントすることができます．
PRの内容を確認してコメントすることを，**レビュー**といいます．

1. PRの一覧（ https://github.com/tomohitoy/oss-gallery-2026/pulls ）を開き，クラスメイトのPRを1つ選びます
2. **Files changed** タブで，追加されたファイルの中身を見ます
3. 本文に書かれているURLを開いて，サイトを見てみます
4. **Conversation** タブのいちばん下のコメント欄にコメントを書いて，**Comment** ボタンをクリックします

コメントを書くときのコツは，次の3つです．
- **よかった点を，具体的に伝える**（「いいですね」だけでなく，どこがどうよかったのか）
- **気づいた点は，提案の形で伝える**（「ここがだめ」ではなく，「こうすると，もっとよくなりそうです」）
- **ていねいな言葉で書く**（相手は，画面の向こうにいるクラスメイトです）

コメントの例：
```markdown
サイトを拝見しました！
見出しの色と背景の色の組み合わせが落ち着いていて，とても読みやすかったです．
スマートフォンで見たとき，自己紹介の文章が画面の端ぎりぎりまで広がっていたので，
左右に少し余白があると，さらに読みやすくなりそうです．
```

> **コメントも，全世界から見えます**．ここで書いたコメントは，あなたのGitHubアカウントの活動として残ります．
> コメントしたPRのURLを，演習課題にあわせて提出してください（加点の対象になります）．

### 手元で一覧ページを作ってみる
一覧ページは，`scripts/build.sh` というスクリプトが，`members` フォルダのファイルから作っています．
手元でも同じページを作って，自分のカードがどう表示されるかを確かめられます．

```bash
bash scripts/build.sh
```

Macの場合：
```bash
open _site/index.html
```

Windows（Git Bash）の場合：
```bash
start _site/index.html
```

作られた `_site` フォルダは，`.gitignore` に書かれているので，`git status` には表示されません（コミットもされません）．
エディタで `scripts/build.sh` を開いて，どうやってページを作っているのかを読んでみるのもおすすめです．

### マージされたあと：Fork と手元を最新にする
PRがマージされると，教員のリポジトリ（upstream）の `main` は先に進みます．
しかし，あなたの Fork の `main` と，手元の `main` は，Fork したときのままです．

まず，GitHubの自分の Fork を最新にします．

1. 自分の Fork（`https://github.com/ユーザー名/oss-gallery-2026`）を開きます
2. ファイルの一覧の上に，`This branch is 5 commits behind tomohitoy/oss-gallery-2026:main.` のように表示されています
3. **Sync fork** → **Update branch** をクリックします

次に，手元のリポジトリも最新にします．

```bash
git switch main
```
```bash
git pull origin main
```

`git pull` は，リモートの新しいコミットを取ってきて，いまいるブランチに取りこむコマンドです．

```bash
ls members
```
```bash
git log --oneline -5
```

クラスメイトのファイルが増えていて，`Merge pull request #12 from taku-shoku/add-my-site` のようなコミットが並んでいるはずです．

### upstream を登録する
手元のリポジトリには，`origin`（自分の Fork）しか登録されていません．
大本のリポジトリを `upstream` という名前で登録しておくと，GitHubの画面を使わずに，大本の最新の状態を直接取ってこられます．

```bash
git remote add upstream git@github.com:tomohitoy/oss-gallery-2026.git
```
```bash
git remote -v
```

表示例：
```
origin	git@github.com:taku-shoku/oss-gallery-2026.git (fetch)
origin	git@github.com:taku-shoku/oss-gallery-2026.git (push)
upstream	git@github.com:tomohitoy/oss-gallery-2026.git (fetch)
upstream	git@github.com:tomohitoy/oss-gallery-2026.git (push)
```

```bash
git fetch upstream
```
```bash
git log --oneline -5 upstream/main
```

> `upstream` には `(push)` も表示されますが，書きこむ権限がないので，push はできません．

### 使い終わったブランチを削除する
マージされたら，`add-my-site` ブランチの役目は終わりです．
PRのページに表示される **Delete branch** ボタンで，GitHubの自分の Fork にあるブランチを削除できます．

手元のブランチは，上の「Fork と手元を最新にする」で `main` を最新にしたあとに，第6回の発展で学んだコマンドで削除できます．

```bash
git branch -d add-my-site
```

> ブランチを削除しても，マージされたコミットは `main` の履歴に残っています．

---

## 参考資料
- [GitHub Docs - フォークについて](https://docs.github.com/ja/pull-requests/collaborating-with-pull-requests/working-with-forks/about-forks)
- [GitHub Docs - pull request について](https://docs.github.com/ja/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)
- [GitHub Docs - フォークからプルリクエストを作成する](https://docs.github.com/ja/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork)
- [GitHub Docs - フォークを同期する](https://docs.github.com/ja/pull-requests/collaborating-with-pull-requests/working-with-forks/syncing-a-fork)
- [GitHub Docs - プロジェクトへのコントリビューション](https://docs.github.com/ja/get-started/exploring-projects-on-github/contributing-to-a-project)
- [Pro Git - GitHub：プロジェクトへの貢献](https://git-scm.com/book/ja/v2/GitHub-%E3%83%97%E3%83%AD%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%81%B8%E3%81%AE%E8%B2%A2%E7%8C%AE)

お疲れさまでした！はじめてのプルリクエストを送れましたか？

### 次回までの準備
- 送ったPRは，Close せずに，そのままにしておいてください
- PRにコメントがついたら，「修正を頼まれたら」を見て対応しましょう（GitHubに登録したメールアドレスに，お知らせが届きます）
- Fork した `oss-gallery-2026` リポジトリは，削除しないでください
