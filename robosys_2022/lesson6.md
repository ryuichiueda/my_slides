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
        ```bash
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
    ```bash
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

---

### 基本事項

* シェルは、`変数名=文字列`で、文字を記憶
* `${変数名}`で値に置換
    * 例:
    ```bash
    $ X=我々は宇宙人だ   # 変数のセット
    $ echo ${X}ぜ！      # 変数を値に
    我々は宇宙人だぜ！
    $ echo $X            # {}は省略できる（こともある）
    我々は宇宙人だ
    $ echo '$X' # 値にしないときはシングルクォートで囲う（``と混同注意）
    $X
    ```
    * `{}`が省略できない例:
    ```bash
    $ echo ${X}Z
    我々は宇宙人だZ
    $ echo $XZ
                         #変数「XZ」と解釈されるのでなにも出力されない
    ```


---

### コマンドの出力を変数に

* `変数名=$(コマンド)`
