# ロボットシステム学

## 第10回: ROSシステムのテスト

千葉工業大学 上田 隆一

<br />

<p style="font-size:50%">
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">
<img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a>
</p>

---

## 今日やること

1. 第8回の内容のコードをGitHubから取り出す
  * 第9回でないので注意
2. 第8回のlistener、talkerの動作を確認するテストを書く
  * シェルスクリプトで（非標準かもしれない）
3. GitHub Actionsでテストを動かす
<br /> 　
<div style="font-size:90%">
注意: 第1回の予告での第10回と第11回を入れ替えました<br />
（テストを書いてからのほうがコードを変更しやすいので）
</div>

---

## 1. 第8回のコードの取り出し

* ちゃんとGitHubでブランチを分けてましたか？ or コミットしてましたか？
    * ブランチを分けていない場合（過去のコミットから取り出し）
        ```bash
        $ git log
        （中略）
          commit 3291472c08ad9407217b8ebf54b971ea4eefe7d6
          Author: Ryuichi Ueda <ryuichiueda@gmail.com>
          Date:   Tue Oct 4 11:18:56 2022 +0900
          
              Change for using Person
          
          commit a906a47eda6962306edc91f3e72175940d2a7505    <- これっぽい
          Author: Ryuichi Ueda <ryuichiueda@gmail.com>
          Date:   Tue Oct 4 11:01:22 2022 +0900
          
              Update
        （中略）
        $ git checkout a906a47                 <- コミットをチェックアウト
        $ git switch -c lesson8                <- 新たにブランチを作成
        Switched to a new branch 'lesson8'     <- このあと動作確認しましょう
        ```


