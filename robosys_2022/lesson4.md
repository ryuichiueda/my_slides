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
