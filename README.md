# Gitの基本概念と実践記録

## はじめに

この学習では、Gitの基本概念と基本コマンドについて調べ、実際に練習用リポジトリを作成して操作した。

今回実践した内容は次のとおり。

- ローカルリポジトリの作成
- ファイルの作成
- `add`と`commit`
- ブランチの作成と切り替え
- ブランチの統合
- コンフリクトの発生と解消
- GitHubへの`push`
- GitHubからの`pull`

---

# 1. Gitの基本概念

## 1-1. Gitとは

Gitとは、ファイルの変更履歴を記録・管理するための仕組みである。

Gitを使うと、次のようなことができる。

- ファイルの変更履歴を保存する
- 過去の変更を確認する
- 複数の作業をブランチに分ける
- GitHubを通して変更内容を共有する
- 複数人で同じプロジェクトを開発する

---

## 1-2. 作業ファイル

作業ファイルとは、現在パソコン上で作成・編集しているファイルのことである。

ファイルを編集しただけでは、変更はGitの履歴に保存されない。

変更を履歴に保存するには、`git add`と`git commit`を使用する。

---

## 1-3. ステージングエリア

ステージングエリアとは、次のコミットに含める変更を一時的に置く場所である。

次のコマンドで、変更したファイルをステージングエリアへ追加する。

```bash
git add ファイル名
```

---

## 1-4. ローカルリポジトリ

ローカルリポジトリとは、自分のパソコン内に保存されるGitのリポジトリである。

`git commit`を実行すると、変更内容がローカルリポジトリの履歴として保存される。

---

## 1-5. リモートリポジトリ

リモートリポジトリとは、GitHubなどのインターネット上に保存されたリポジトリである。

ローカルの変更は`git push`でリモートリポジトリへ送信できる。

リモートリポジトリの変更は`git pull`でローカルへ取り込める。

---

## 1-6. ブランチ

ブランチとは、作業履歴を枝分かれさせる仕組みである。

新しい機能や修正を別のブランチで行うことで、元のコードに直接影響を与えずに作業できる。

作業が完了したら、`git merge`で別のブランチの変更を統合する。

---

## 1-7. Gitの基本的な流れ

Gitを使った開発では、一般的に次の順番で作業する。

```text
1. リモートの最新状態を取得
        ↓
2. 作業用ブランチを作成
        ↓
3. ファイルを編集
        ↓
4. 変更内容を確認
        ↓
5. 変更をステージング
        ↓
6. 変更をコミット
        ↓
7. mainの最新状態を取り込む
        ↓
8. ブランチをマージ
        ↓
9. コンフリクトがあれば解消
        ↓
10. GitHubへプッシュ
        ↓
11. 不要になったブランチを削除
```

---

### 1. リモートリポジトリの最新状態を取得する

作業を始める前に、現在のブランチを確認する。

```bash
git branch
```

`main`以外のブランチにいる場合は、`main`へ切り替える。

```bash
git switch main
```

次に、GitHub上の最新状態をローカルへ取り込む。

```bash
git pull origin main
```

すでにローカルの`main`とGitHubの`main`が関連付けられている場合は、次のコマンドだけでもよい。

```bash
git pull
```

最初に`pull`しておくことで、古い状態のコードを使って作業を始めることを防げる。

---

### 2. 作業用ブランチを作成する

通常は、`main`ブランチを直接編集せず、作業内容ごとに新しいブランチを作成する。

```bash
git switch -c ブランチ名
```

例：

```bash
git switch -c feature
```

このコマンドは、次の2つの操作を同時に行う。

1. `feature`ブランチを作成する
2. 作成した`feature`ブランチへ切り替える

現在のブランチを確認する。

```bash
git branch
```

表示例：

```text
* feature
  main
```

`*`が付いているブランチが、現在作業しているブランチである。

従来の`checkout`を使う場合は、次のように入力する。

```bash
git checkout -b feature
```

---

### 3. ファイルを作成・編集する

作業用ブランチ上で、必要なファイルを作成または編集する。

今回の練習では、次のコマンドで`memo.txt`に文章を追加した。

```bash
echo "featureブランチで編集しました" >> memo.txt
```

Flutter開発の場合は、エディタを使ってDartファイルなどを編集する。

例：

```text
lib/main.dart
lib/pages/home_page.dart
pubspec.yaml
```

この段階では、ファイルを変更しただけであり、まだGitの履歴には保存されていない。

---

### 4. 変更内容を確認する

変更したファイルを確認する。

```bash
git status
```

表示例：

```text
Changes not staged for commit:
  modified: memo.txt
```

`Changes not staged for commit`は、ファイルは変更されているが、まだステージングされていない状態を表す。

実際にどの部分が変更されたか確認する場合は、次のコマンドを使用する。

```bash
git diff
```

コミットする前に、不要な変更や間違いが含まれていないか確認する。

---

### 5. 変更をステージングする

コミットに含めたいファイルをステージングエリアへ追加する。

```bash
git add ファイル名
```

例：

```bash
git add memo.txt
```

複数の変更をまとめて追加する場合は、次のコマンドを使用できる。

```bash
git add .
```

ただし、`git add .`はすべての変更を追加するため、不要なファイルまで含まれていないか`git status`で確認する必要がある。

ステージング後の状態を確認する。

```bash
git status
```

表示例：

```text
Changes to be committed:
  modified: memo.txt
```

これは、`memo.txt`が次のコミットに含まれる状態を表している。

---

### 6. 変更をコミットする

ステージングした変更をローカルリポジトリの履歴として保存する。

```bash
git commit -m "変更内容の説明"
```

例：

```bash
git commit -m "featureブランチでメモを更新"
```

コミットメッセージには、何を変更したのか分かる説明を書く。

良い例：

```bash
git commit -m "ログイン画面を追加"
git commit -m "入力チェックの不具合を修正"
git commit -m "ホーム画面のレイアウトを調整"
```

内容が分かりにくい例：

```bash
git commit -m "変更"
git commit -m "修正"
git commit -m "作業"
```

コミット後は、履歴を確認する。

```bash
git log --oneline
```

---

### 7. 必要に応じて作業とコミットを繰り返す

1回の作業ですべてをまとめてコミットする必要はない。

作業内容を適切な単位に分けて、次の流れを繰り返す。

```text
ファイルを編集
    ↓
git statusで確認
    ↓
git add
    ↓
git commit
```

例えばFlutterアプリの場合は、次のように分けられる。

```text
1回目：画面を作成
2回目：ボタンの処理を追加
3回目：表示の不具合を修正
```

履歴を分けておくと、後から変更内容を確認しやすくなる。

---

### 8. mainブランチの最新状態を確認する

作業中に、他の人がGitHub上の`main`を更新している可能性がある。

そのため、統合する前に`main`の最新状態を取得する。

まず、作業中の変更がすべてコミットされていることを確認する。

```bash
git status
```

次の表示になっていれば、すべてコミットされている。

```text
nothing to commit, working tree clean
```

`main`へ切り替える。

```bash
git switch main
```

GitHub上の最新状態を取得する。

```bash
git pull origin main
```

---

### 9. 作業用ブランチをmainへマージする

最新状態になった`main`へ、作業用ブランチの変更を統合する。

現在地が`main`であることを確認する。

```bash
git branch
```

表示例：

```text
  feature
* main
```

次に、`feature`ブランチを`main`へ統合する。

```bash
git merge feature
```

ここで重要なのは、`git merge feature`は「現在いるブランチへ`feature`を統合する」という意味である。

そのため、先に`main`へ切り替えてから実行する必要がある。

```text
featureで作業
    ↓
mainへ切り替える
    ↓
mainにfeatureをマージ
```

正しい操作：

```bash
git switch main
git merge feature
```

---

### 10. マージに成功した場合

変更箇所が重なっていなければ、Gitが自動的に統合する。

表示例：

```text
Updating b59c42d..8ffac42
Fast-forward
memo.txt | 1 +
1 file changed, 1 insertion(+)
```

`Fast-forward`は、履歴をそのまま前へ進める形でマージできたことを表す。

マージ後は履歴を確認する。

```bash
git log --oneline --graph --all
```

---

### 11. コンフリクトが発生した場合

複数のブランチで同じファイルの同じ場所を異なる内容に変更すると、コンフリクトが発生する場合がある。

表示例：

```text
CONFLICT (content): Merge conflict in memo.txt
Automatic merge failed; fix conflicts and then commit the result.
```

これは、Gitがどちらの変更を採用すればよいか判断できなかった状態である。

ファイルを開くと、次のような目印が表示される。

```text
<<<<<<< HEAD
mainブランチの内容
=======
featureブランチの内容
>>>>>>> feature
```

それぞれの意味は次のとおり。

- `<<<<<<< HEAD`から`=======`まで：現在いるブランチの内容
- `=======`から`>>>>>>> feature`まで：統合するブランチの内容

どちらを残すか、または両方を残すか判断し、ファイルを手動で修正する。

修正後は、次のような目印をすべて削除する。

```text
<<<<<<< HEAD
=======
>>>>>>> feature
```

修正したファイルをステージングする。

```bash
git add memo.txt
```

コンフリクトの解消をコミットする。

```bash
git commit -m "コンフリクトを解消"
```

状態を確認する。

```bash
git status
```

次の表示になれば、コンフリクトの解消とマージが完了している。

```text
nothing to commit, working tree clean
```

---

### 12. マージしたmainをGitHubへプッシュする

ローカルの`main`へマージしただけでは、GitHub上の`main`にはまだ反映されていない。

GitHubへ送信するために`push`を実行する。

```bash
git push origin main
```

すでに送信先が設定されている場合は、次のコマンドだけでもよい。

```bash
git push
```

これで、ローカルの`main`とGitHub上の`main`が同じ状態になる。

```text
ローカルのfeature
        ↓ merge
ローカルのmain
        ↓ push
GitHub上のmain
```

---

### 13. 不要になったブランチを削除する

`feature`の変更が`main`へ正しく統合されたことを確認したら、ローカルの作業用ブランチを削除できる。

```bash
git branch -d feature
```

`-d`は、マージ済みのブランチを安全に削除するための指定である。

ブランチを削除しても、`main`へ統合された変更やコミット履歴は消えない。

削除後はブランチ一覧を確認する。

```bash
git branch
```

表示例：

```text
* main
```

---

## GitHubを使った基本的な一連のコマンド

作業開始からGitHubへの送信までをまとめると、次のようになる。

```bash
# 1. mainへ移動
git switch main

# 2. GitHubの最新状態を取得
git pull origin main

# 3. 作業用ブランチを作成して移動
git switch -c feature

# 4. ファイルを編集したあと、状態を確認
git status
git diff

# 5. 変更をステージング
git add ファイル名

# 6. 変更をコミット
git commit -m "変更内容の説明"

# 7. mainへ戻る
git switch main

# 8. mainの最新状態を再度取得
git pull origin main

# 9. 作業用ブランチをmainへ統合
git merge feature

# 10. GitHubへ送信
git push origin main

# 11. 統合済みのブランチを削除
git branch -d feature
```

コンフリクトが発生した場合は、マージとプッシュの間に解消作業を行う。

```bash
# ファイルを手動で修正したあと
git add ファイル名
git commit -m "コンフリクトを解消"
git push origin main
```

---

## 今回の実習で行った流れ

今回の練習では、次の順番で操作した。

```text
git-practiceを作成
        ↓
git initでローカルリポジトリ化
        ↓
memo.txtを作成
        ↓
git addでステージング
        ↓
git commitで履歴を保存
        ↓
featureブランチを作成して切り替え
        ↓
feature側でmemo.txtを編集してコミット
        ↓
mainブランチへ切り替え
        ↓
main側でも同じ場所を編集してコミット
        ↓
git merge featureを実行
        ↓
コンフリクトが発生
        ↓
両方の内容を残して手動で解決
        ↓
解決したファイルをaddしてcommit
        ↓
GitHubのリモートリポジトリを登録
        ↓
git pushでGitHubへ送信
        ↓
GitHub上でmemo.txtを編集
        ↓
git pullでローカルへ取り込み
```

---

## 個人作業とチーム開発の違い

今回の練習では、ローカルで`feature`を`main`へマージしてから、GitHubへプッシュした。

```text
ローカルでブランチを作る
    ↓
ローカルでコミット
    ↓
ローカルでmainへマージ
    ↓
mainをGitHubへプッシュ
```

チーム開発では、作業用ブランチを先にGitHubへプッシュし、GitHub上でプルリクエストを作成する方法がよく使われる。

```text
ローカルでブランチを作る
    ↓
ローカルでコミット
    ↓
作業用ブランチをGitHubへプッシュ
    ↓
GitHubでプルリクエストを作成
    ↓
内容を確認してもらう
    ↓
GitHub上でmainへマージ
    ↓
ローカルでgit pull
```

作業用ブランチをGitHubへ初めて送る場合は、次のように入力する。

```bash
git push -u origin feature
```

GitHub上でマージされたあと、ローカルの`main`を最新状態にする。

```bash
git switch main
git pull origin main
```

---

### まとめ

Gitの基本的な作業は、次の4段階に分けて考えると理解しやすい。

### 変更を保存する段階

```text
ファイルを編集
    ↓
git add
    ↓
git commit
```

### ブランチを統合する段階

```text
git switch main
    ↓
git pull
    ↓
git merge feature
```

### GitHubへ共有する段階

```text
git push
```

### GitHubの変更を取り込む段階

```text
git pull
```

`commit`はローカルへ履歴を保存する操作、`push`はその履歴をGitHubへ送る操作である。

`merge`は別のブランチの変更を現在のブランチへ統合する操作であり、`pull`はGitHub上の変更をローカルへ取り込む操作である。

GitHub側の変更は、`git pull`でローカルリポジトリへ取り込む。

---

# 2. Gitの基本コマンド

## git add

変更したファイルをステージングエリアへ追加する。

```bash
git add ファイル名
```

例：

```bash
git add memo.txt
```

すべての変更を追加する場合は、次のように入力する。

```bash
git add .
```

---

## git commit

ステージングエリアへ追加した変更を、ローカルリポジトリの履歴として保存する。

```bash
git commit -m "変更内容の説明"
```

例：

```bash
git commit -m "最初のメモを追加"
```

---

## git push

ローカルリポジトリのコミットをGitHubへ送信する。

```bash
git push
```

初回は、送信先とブランチを指定する場合がある。

```bash
git push -u origin main
```

---

## git pull

GitHub上の最新の変更をローカルへ取り込む。

```bash
git pull
```

---

## git branch

ブランチの一覧を確認する。

```bash
git branch
```

新しいブランチを作成する場合は、次のように入力する。

```bash
git branch ブランチ名
```

---

## git checkout

指定したブランチへ切り替える。

```bash
git checkout ブランチ名
```

例：

```bash
git checkout feature
```

現在は、ブランチの切り替え専用として`git switch`も使用できる。

```bash
git switch feature
```

---

## git switch

指定したブランチへ切り替える。

```bash
git switch ブランチ名
```

新しいブランチを作成し、そのブランチへ切り替える場合は、`-c`を付ける。

```bash
git switch -c 新しいブランチ名
```

---

## git merge

指定したブランチの変更を、現在のブランチへ統合する。

```bash
git merge ブランチ名
```

例：

```bash
git merge feature
```

---

## git status

現在のブランチやファイルの変更状態を確認する。

```bash
git status
```

---

## git log

コミット履歴を確認する。

```bash
git log --oneline
```

全ブランチの履歴を図のように確認する場合は、次のように入力する。

```bash
git log --oneline --graph --all
```

---

# 3. コンフリクトについて

## 3-1. コンフリクトとは

コンフリクトとは、複数のブランチで同じファイルの同じ場所を異なる内容に変更したときに発生する競合である。

Gitがどちらの内容を採用すればよいか自動で判断できないため、利用者が手動で解決する必要がある。

---

## 3-2. コンフリクト発生時の表示

コンフリクトが発生すると、ファイル内に次のような目印が追加される。

```text
<<<<<<< HEAD
現在のブランチの内容
=======
統合しようとしたブランチの内容
>>>>>>> feature
```

それぞれの意味は次のとおり。

- `<<<<<<< HEAD`：現在のブランチの内容
- `=======`：変更内容の区切り
- `>>>>>>> feature`：統合しようとしたブランチの内容

---

## 3-3. コンフリクトの解決方法

ファイルの内容を確認し、残したい内容に手動で書き換える。

その後、次のコマンドを実行する。

```bash
git add ファイル名
git commit -m "コンフリクトを解消"
```

これでコンフリクトの解決が履歴として保存される。

---

# 4. 実践記録

## 4-1. Gitの動作確認

最初に、Gitがインストールされているか確認した。

```bash
git --version
```

実行結果：

```text
git version 2.50.1 (Apple Git-155)
```

Gitのバージョンが表示されたため、使用できる状態だと確認できた。

---

## 4-2. 練習用リポジトリの作成

デスクトップに`git-practice`という練習用フォルダを作成した。

```bash
cd ~/Desktop
mkdir git-practice
cd git-practice
git init
```

実行結果：

```text
Initialized empty Git repository in /Users/nanami/Desktop/git-practice/.git/
```

`git init`によって、`git-practice`がローカルリポジトリになった。

---

## 4-3. 練習用ファイルの作成

次のコマンドで`memo.txt`を作成した。

```bash
echo "Gitの練習を始めました" > memo.txt
```

状態を確認した。

```bash
git status
```

`memo.txt`が未追跡ファイルとして表示された。

これは、ファイルは作成されているが、まだGitの記録対象に追加されていない状態である。

---

## 4-4. ファイルをステージングエリアへ追加

次のコマンドを実行した。

```bash
git add memo.txt
```

状態を確認した。

```bash
git status
```

`memo.txt`がコミット予定のファイルとして表示された。

---

## 4-5. 最初のコミット

次のコマンドで変更を保存した。

```bash
git commit -m "最初のメモを追加"
```

履歴を確認した。

```bash
git log --oneline
```

実行結果：

```text
b59c42d 最初のメモを追加
```

状態を確認した。

```bash
git status
```

実行結果：

```text
On branch main
nothing to commit, working tree clean
```

`working tree clean`は、すべての変更がコミットされている状態を表している。

---

## 4-6. featureブランチの作成

`feature`という新しいブランチを作成し、そのブランチへ切り替えた。

```bash
git switch -c feature
```

ブランチ一覧を確認した。

```bash
git branch
```

実行結果：

```text
* feature
  main
```

`*`が付いている`feature`が、現在使用しているブランチである。

---

## 4-7. featureブランチでファイルを変更

`memo.txt`へ文章を追加した。

```bash
echo "featureブランチで編集しました" >> memo.txt
```

変更を保存した。

```bash
git add memo.txt
git commit -m "featureブランチでメモを更新"
```

---

## 4-8. mainブランチへ戻る

次のコマンドで`main`ブランチへ戻った。

```bash
git switch main
```

ファイルの内容を確認した。

```bash
cat memo.txt
```

実行結果：

```text
Gitの練習を始めました
```

`feature`ブランチで追加した文章は、`main`ブランチにはまだ反映されていなかった。

この結果から、ブランチごとに異なるファイルの状態を持てることが確認できた。

---

## 4-9. mainブランチでファイルを変更

`main`ブランチでも、同じファイルに別の文章を追加した。

```bash
echo "mainブランチで編集しました" >> memo.txt
```

変更を保存した。

```bash
git add memo.txt
git commit -m "mainブランチでメモを更新"
```

全ブランチの履歴を確認した。

```bash
git log --oneline --graph --all
```

履歴が`main`と`feature`の2本に分かれていることを確認できた。

---

## 4-10. コンフリクトの発生

`main`ブランチに`feature`ブランチを統合した。

```bash
git merge feature
```

実行結果：

```text
Auto-merging memo.txt
CONFLICT (content): Merge conflict in memo.txt
Automatic merge failed; fix conflicts and then commit the result.
```

両方のブランチで`memo.txt`の同じ場所を異なる内容に変更したため、コンフリクトが発生した。

---

## 4-11. コンフリクトの内容を確認

次のコマンドでファイルの内容を確認した。

```bash
cat memo.txt
```

ファイル内に、現在のブランチと`feature`ブランチの変更内容が表示された。

---

## 4-12. コンフリクトの解消

今回は、両方のブランチの文章を残すことにした。

次のコマンドでファイルを完成形に書き換えた。

```bash
printf "Gitの練習を始めました\nmainブランチで編集しました\nfeatureブランチで編集しました\n" > memo.txt
```

内容を確認した。

```bash
cat memo.txt
```

実行結果：

```text
Gitの練習を始めました
mainブランチで編集しました
featureブランチで編集しました
```

解決したファイルをコミットした。

```bash
git add memo.txt
git commit -m "コンフリクトを解消"
```

状態を確認した。

```bash
git status
```

実行結果：

```text
On branch main
nothing to commit, working tree clean
```

---

## 4-13. 統合後の履歴を確認

次のコマンドで履歴を確認した。

```bash
git log --oneline --graph --all
```

実行結果：

```text
*   4d0e535 コンフリクトを解消
|\
| * 8ffac42 featureブランチでメモを更新
* | e93e5a5 mainブランチでメモを更新
|/
* b59c42d 最初のメモを追加
```

線が途中で分かれて再び合流している部分が、ブランチの分岐と統合を表している。

---

# 5. GitHubとの接続

## 5-1. リモートリポジトリの作成

GitHub上に、次の練習用リポジトリを作成した。

```text
https://github.com/nanami1213school-coder/git-practice
```

ローカルリポジトリへGitHubのURLを登録した。

```bash
git remote add origin https://github.com/nanami1213school-coder/git-practice.git
```

登録内容を確認した。

```bash
git remote -v
```

`origin`は、登録したリモートリポジトリを表す名前である。

---

## 5-2. GitHubへの認証

最初に`git push`を実行した際、GitHubの通常のパスワードではGit操作を認証できなかった。

そのため、GitHub CLIをインストールした。

```bash
brew install gh
```

インストールを確認した。

```bash
gh --version
```

GitHubへログインした。

```bash
gh auth login
```

次の内容を選択した。

```text
GitHub.com
HTTPS
Yes
Login with a web browser
```

ターミナルに表示された一時コードをGitHubの認証画面に入力し、アクセスを許可した。

ログイン状態を確認した。

```bash
gh auth status
```

一時コードやアクセストークンは認証情報なので、他人へ共有したり、公開したりしないように注意する。

---

# 6. pushの実践

ローカルの`main`ブランチをGitHubへ送信した。

```bash
git push -u origin main
```

実行結果：

```text
[new branch] main -> main
branch 'main' set up to track 'origin/main'.
```

ローカルのコミット履歴がGitHubへ送信された。

最初に`-u origin main`を指定したことで、ローカルの`main`とGitHubの`main`が関連付けられた。

次回からは、基本的に次のコマンドだけで送信できる。

```bash
git push
```

---

# 7. pullの実践

GitHub上で`memo.txt`を直接編集し、次の文章を追加した。

```text
GitHub上で編集しました
```

GitHub上の変更をローカルへ取り込んだ。

```bash
git pull
```

実行結果：

```text
Updating 4d0e535..d2bc6eb
Fast-forward
memo.txt | 1 +
1 file changed, 1 insertion(+)
```

ファイルの内容を確認した。

```bash
cat memo.txt
```

実行結果：

```text
Gitの練習を始めました
mainブランチで編集しました
featureブランチで編集しました
GitHub上で編集しました
```

GitHub上で追加した文章がローカルにも表示されたため、`pull`に成功したことを確認できた。

---

# 8. 今回使用したGitコマンド一覧

| コマンド | 内容 |
|---|---|
| `git --version` | Gitのバージョンを確認する |
| `git init` | ローカルリポジトリを作成する |
| `git status` | 現在の変更状態を確認する |
| `git add ファイル名` | 変更をステージングエリアへ追加する |
| `git commit -m "説明"` | 変更を履歴として保存する |
| `git log --oneline` | コミット履歴を簡潔に表示する |
| `git log --oneline --graph --all` | 全ブランチの履歴を図のように表示する |
| `git branch` | ブランチ一覧を確認する |
| `git switch -c 名前` | 新しいブランチを作成して切り替える |
| `git switch 名前` | ブランチを切り替える |
| `git checkout 名前` | ブランチを切り替える |
| `git merge 名前` | 指定したブランチを現在のブランチへ統合する |
| `git remote add origin URL` | リモートリポジトリを登録する |
| `git remote -v` | 登録したリモートリポジトリを確認する |
| `git push` | ローカルのコミットをGitHubへ送る |
| `git pull` | GitHubの変更をローカルへ取り込む |

---

# 9. ターミナルの関連キー操作

以下はGitコマンドではなく、ターミナルで使用するキー操作である。

| キー操作 | 内容 |
|---|---|
| `Control + C` | 実行中の処理を中止する |
| `Control + Q` | 一時停止したターミナルの表示を再開する |
| `Control + S` | ターミナルの表示を一時停止する場合がある |
| `Control + L` | ターミナルの画面表示を整理する |
| `Control + A` | 入力中の行の先頭へ移動する |
| `Control + E` | 入力中の行の末尾へ移動する |
| `Control + U` | カーソルより前の入力を削除する |
| `Control + K` | カーソルより後ろの入力を削除する |
| `Control + W` | カーソル直前の単語を削除する |
| 上矢印キー | 過去に実行したコマンドを表示する |
| 下矢印キー | コマンド履歴を次へ進める |
| `Tab` | コマンドやファイル名の入力を補完する |

ターミナルが反応しなくなったように見える場合は、誤って`Control + S`を押し、表示が一時停止している可能性がある。

その場合は、次のキー操作で表示を再開できる。

```text
Control + Q
```

実行中の処理を中止したい場合は、次のキー操作を使用する。

```text
Control + C
```

---

# 10. 実践を通して分かったこと

今回の実践を通して、Gitはファイルそのものを保存するだけではなく、ファイルの変更履歴を管理する仕組みであることが分かった。

基本的な流れは、次のとおりである。

1. ファイルを作成・編集する
2. `git add`で記録する変更を選ぶ
3. `git commit`でローカルに履歴を保存する
4. `git push`でGitHubへ送信する
5. GitHub側の変更を`git pull`でローカルへ取り込む

また、ブランチを使用すると、作業を分けて進められることも確認できた。

複数のブランチで同じ場所を異なる内容に変更すると、コンフリクトが発生する場合がある。その場合は、ファイルの内容を確認して手動で修正し、再度`git add`と`git commit`を実行する必要がある。

今回の実践により、Gitの基本的な変更管理、ブランチ操作、コンフリクト解消、GitHubとの連携を一通り体験できた。

---
今後メモを更新するときは、GitHub上で鉛筆マークを押して編集してもよいし、ローカルでREADME.mdを編集して次のように送ることもできる。
```bash
git add README.md
git commit -m "Git学習メモを更新"
git push
```
