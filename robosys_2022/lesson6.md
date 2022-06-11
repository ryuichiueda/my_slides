# ロボットシステム学

## 第6回: ソフトウェアのテスト

千葉工業大学 上田 隆一

<br />

<p style="font-size:50%">
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">
<img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a>
</p>

---

## 今日やること

* コマンドの終了ステータス
* シェルの変数
* 簡単なシェルスクリプト
* 初歩的なテスト

---

## 1. コマンドの終了ステータス

---

### コマンドの正常終了・異常終了

* コマンドは人に対してだけでなく、<br />シェルにエラーの有無を伝達
    * <span style="color:red">終了ステータス</span>と呼ばれる整数値で
* シェルはコマンドの終了ステータスを記録
    * <span style="color:red">`$?`</span>という変数に
    * 例: `ls`の出力
        ```
$ ls /etc/passwd   # 存在するファイルをls
/etc/passwd
$ echo $?
0                  # $?という変数にゼロが入っている。
$ ls aaaaaaaa      # 存在しないファイルをls
ls: 'aaaaaaaa' にアクセスできません: そのようなファイルやディレクトリはありません
$ echo $?
2                  # ゼロでない値が入る。
        ```

---

### `plus_stdin`の終了ステータス

* これまで書いてきた`plus_stdin`も終了ステータスを<br />シェルに伝達
    * Pythonが裏でやっているので、任せておいて問題ない
    ```
$ seq 3 | ./plus_stdin    # 正常な入力
6
$ echo $?
0
$ echo あ | ./plus_stdin  # ひらがなを入力してエラーを起こさせる
（エラーの表示。省略。）
$ echo $?
1
    ```

なんのために終了ステータスがあるか: あとで説明

---

## 2. シェルの変数

