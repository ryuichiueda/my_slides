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
        * リポジトリ: あるソフトウェアに関するファイルの集まり
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
