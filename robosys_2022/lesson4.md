# ロボットシステム学

## 第4回: <span style="text-transform:none">GitとGitHub</span>

千葉工業大学 上田 隆一

<br />

<p style="font-size:50%">
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">
<img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a>
</p>

---

## 今日やること

* GitとGitHubを使う

---

## <span style="text-transform:none">Git</span>

* 版管理（バージョン管理）システム
    * ファイルの変更履歴を管理するためのシステム
    * コードや文章を書くときは必須と言っても過言ではない<br />　
* Linus Torvalds が作成
    * Linuxの共同開発のため

---

## <span style="text-transform:none">Git</span>のインストール

* やること
    1. `sudo apt install git`等
        * 最近はデフォルトで使える環境が多い
    2. ユーザの設定

```bash
$ sudo apt install git
###自身の名前とe-mail アドレスを記録しておく###
$ git config --global user.name "Ryuichi Ueda"
$ git config --global user.email "ueda@hogehoge.com"
###エディタも登録しておくとよい###
$ git config --global core.editor vim
###確認###
$ cat .gitconfig
[user]
name = Ryuichi Ueda
email = ueda@hogehoge.com
[core]
editor = vim
```

---

## <span style="text-transform:none">GitHub</span>

* Gitを利用したサービス
    * 「リポジトリ」のホスティングと公開、コミュニケーション
        * <span style="color:red">リポジトリ</span>: あるソフトウェアに関するファイルの集まり
    * 公開しないリポジトリも作成可能<br />　
* 利用方法
    * ウェブサイト([https://github.co.jp/ ](https://github.co.jp/))
    * コマンドライン
        * `git`コマンド
        * `gh`コマンド（本講義では扱わず）

---

## <span style="text-transform:none">GitHub</span>でのアカウント作成

<span style="font-size:60%">注意: 文言等はよく変更されるので、基本的にサイトの英語を読んで手続きを</span>

1. トップページで"Sign up"か"GitHubに登録する"を押す
2. ユーザ名、email アドレス、パスワードを決めて<br />"Create account"を押す
    * ユーザ名は恥ずかしくないものを！
3. 画面指示に従って手続き
    * プランを選ぶときに"Free"が選択されているのを確認<br />$\rightarrow$"Finish sign up"
4. 登録したメールアドレスに確認メールが届く<br />$\rightarrow$指示にしたがう
5. 鍵の登録（次ページ）
6. （必要ならば）ファイアウォール対策

---

## 鍵の設定（鍵の作成）

* 手元のPCとGitHubとの通信を暗号化するために、<span style="color:red">公開鍵</span>をGitHubに登録
    * 手元のPCには<span style="color:red">秘密鍵</span>を持っておく
        * 秘密鍵は文字通り秘密にして他人に見せたり触れたりさせない<br />　
* 鍵の作り方
    ```bash
    $ ssh-keygen
    （いろいろ聞かれるけどすべてEnterで大丈夫）
    $ ls ~/.ssh/                   #確認
    id_rsa      id_rsa.pub         #この2つのファイルがあれば大丈夫
    ```
    * `id_rsa.pub`の方が公開鍵

---

## 鍵の設定（<span style="text-transform:none">GitHub</span>での作業）

* 右上のユーザのアイコンを押す$\rightarrow$Settings$\rightarrow$SSH and GPG keys$\rightarrow$New SSH key
    * titleはなんでもいいので鍵の名前を入れる
        * 例: 「WSL」とか「Ubuntu Note」とか
    * Keyに`id_rsa.pub`の中身を貼り付け
        * エディタや端末で開いてマウス等でコピーして貼り付け
<img width="70%" src="./figs/ssh_keys.png" />

---

## ファイアウォール回避の設定

* ホーム下の`.ssh/config`というファイルに次のように記述
    ```bash
    $ cat ~/.ssh/config
    ・・・
    
    Host github.com
          Hostname ssh.github.com
          User git
          Port 443
          IdentityFile ~/.ssh/id_rsa
    
    ・・・
    ```
    * SSHでデフォルトの22番ポートではなく<br />HTTPSの443番ポートを使う設定
        * 参考: https://docs.github.com/ja/authentication/troubleshooting-ssh/using-ssh-over-the-https-port


---

## <span style="text-transform:none">GitHub</span>へのコードの保存

* やること
    * これまで講義で作ってきたコードをGitHubにアップロード
    * コードを消しちゃった人は前回の`plus_stdin`を作りましょう
        * `plus_stdin`
            ```python
            #!/usr/bin/python3
            import sys
            
            ans = 0.0
            for line in sys.stdin:
                ans += float(line)
            
            print(ans)
            ```

---

## リポジトリの作成

GitHubに1つ作ってみましょう

* GitHubのサイトでの操作
  * 右上のアカウントのアイコン横の"+"マークを押して、<br />"New repository"を選択
  * 必要事項を記入
    * 名前: robosys202x
    * Description: 説明を適当に
    * Publicで
    * "Add a README file"にチェック
    * ライセンスは別の回で追加します
  * "Create repository"ボタンを押す
* ウェブ画面にリポジトリの画面
  * `README.md`がひとつ存在したリポジトリができる

---

## リポジトリを手元にコピー

* リポジトリの画面の"Code"をクリック
* "SSH"を選択してURLをコピー
  * クリップボードのアイコンをクリックするとコピーできる
* リポジトリをコピーしたいディレクトリで次の操作
    * この操作を<span style="color:red">クローン</span>と言う
        ```bash
        $ git clone <さっきクリップボードにコピーした文字列をペースト>
        Cloning into 'robosys2022'...
        remote: Enumerating objects: 3, done.
        remote: Counting objects: 100% (3/3), done.
        remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
        Receiving objects: 100% (3/3), done.
        $ cd robosys2022/
        $ ls -a
        .  ..  .git  README.md
        ```
    * <span style="color:red">注意: </span>鍵、`.git/config`の設定が失敗しているとエラー

---

## リポジトリにコードを追加<br />1: <span style="text-transform:none">git add</span>

* プログラム`plus_stdin`を一つ置く
    ```bash
    $ ls
    README.md  plus_stdin
    ```
* <span style="color:red">`git add`</span>で記録の対象として選択
    * <span style="color:red">ステージングエリア</span>というところに記録される
        ```bash
        $ git add plus_stdin
        $ git status             #ステージングエリアの確認
        ブランチ main
        Your branch is up to date with 'origin/main'.
        
        コミット予定の変更点:
          (use "git restore --staged <file>..." to unstage)
        	new file:   plus_stdin
        ```

---

## リポジトリにコードを追加<br />2: <span style="text-transform:none">git commit</span>

* <span style="color:red">`git commit`</span>でステージングエリアの情報をリポジトリに反映
    * この時点で、手元のリポジトリに`plus_stdin`の記録が残る
    * `git commit`で作った1つの記録を<span style="color:red">コミット</span>と呼ぶ
        ```bash
        $ git commit -m "Add a command" #git commit -m "何をしたか短く"
        [main fa8aab8] Add a command
         1 file changed, 8 insertions(+)
         create mode 100755 plus_stdin
        $ git log -n 1                 #最新のコミット1件を表示
        commit fa8aab8a2ade8cd33823f488fbb1bbec6d981260 (HEAD -> main)
        Author: Ryuichi Ueda <ryuichiueda@gmail.com>
        Date:   Tue Dec 7 16:58:54 2021 +0900
	　
            Add a command
        ```

---

## <span style="text-transform:none">GitHub</span>への反映

* 手元のリポジトリをGitHubのリポジトリへ転送
    * <span style="color:red">プッシュ</span>と呼ぶ
        * 手元（<span style="color:red">ローカルリポジトリ</span>からGitHub（<span style="color:red">リモートリポジトリ</span>）へ
    * コマンドは<span style="color:red">`git push`</span> 
        ```bash
        $ git push   #git push origin mainと打たないといけない場合もある
        Enumerating objects: 4, done.
        Counting objects: 100% (4/4), done.
        Delta compression using up to 16 threads
        Compressing objects: 100% (3/3), done.
        Writing objects: 100% (3/3), 373 bytes | 373.00 KiB/s, done.
        Total 3 (delta 0), reused 0 (delta 0)
        To github.com:ryuichiueda/robosys2022.git
           68d342f..fa8aab8  main -> main
        ```
    * プッシュしたらGitHub側で反映されたことを確認のこと

---

## <span style="text-transform:none">GitHub</span>を利用した開発

* GitHubにコードをアップした時点で様々な利点
    * 自分のコードを紛失する可能性が極めて低く
    * 混乱せずに様々な環境で開発可能に
    * 自分の力を見せることが可能に
        * たとえ学科内だと平凡でも、世の中的にはコードが書けるだけで少数派<br />　
* 面倒なこと: 少々責任が伴う
    * ライセンス等の整備（また別の回で）
    * <span style="color:red">使えないものを使えると言って置かない</span>
        * 他の人が使うかもしれない

---

## 動くものを残しながらの開発

* よくあるケース
    * 改良しようと結構手を加えたらコードが動かなくなった<br />　
* どうする？
    * そのままGitHubにpushすると他の人がコードを使えなくなる
    * GitHubにpushしないで放置すると作業の記録が残せない

<span style="color:red">$\Rightarrow$ブランチを分ける</span>

---

## ブランチ

* リポジトリの内容を枝分かれして開発を進める
  * ブランチ = 枝
* 今のところブランチは「main」だけ
  ```bash
  $ git branch
  * main          #ブランチはmainだけ。「*」は選択状態を表現
  ```
* 開発用ブランチを作りましょう
  ```bash
  $ git checkout -b dev
  Switched to a new branch 'dev'
  $ git branch
  * dev               #devブランチができて、devが選択状態に
    main
  ```

---

## <span style="text-transform:none">dev</span>ブランチでの開発

ついでにPythonの文法の勉強

* やること1
    * `plus_stdin`について、整数の入力を整数に変換するよう改良
        * 注意: 改良じゃないかもしれません
        * <span style="color:red">例外処理</span>をしてみましょう
            * 失敗しそうな処理を<span style="color:red">`try`</span>で囲む
	    * 下に<span style="color:red">`except`</span>のブロックを作って例外処理

```python
#!/usr/bin/python3
import sys

ans = 0   #もともと0.0だったのを0に変更
for line in sys.stdin:
    try: 
        ans += int(line)   #intは文字列を整数に（失敗すると例外発生）
    except:
        ans += float(line)

print(ans)
```
