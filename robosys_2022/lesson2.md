# ロボットシステム学

## 第2回: <span style="text-transform:none">Linux環境での<br />プログラミング</span>

千葉工業大学 上田 隆一

<br />

<p style="font-size:50%">
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">
<img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a>
</p>

---

## 今日やること

* Pythonでプログラムを動かす
* プログラムがLinux上でどう動いているのか理解
    * こっちの方が大事かもしれない

---

## 最初のコード

* Vimで（emacsに慣れている人はemacsで）書きましょう
    * ファイル名は`hello.py`
```bash
$ vi hello.py
```
    * `hello.py`の中身
```python
print("hello")              #文字列を出力
print(3.14)                 #数字を出力
```
        * 「`#`」より後ろはコメント
* 実行
    * <span style="color:red">`python3`</span>コマンドに読み込ませる
        ```bash
        $ python3 hello.py
        hello
        3.14
        ```

---

## <span style="text-transform:none">hello.py</span>のコード

* 関数
    * `名前(引数, 引数, ...)`<br />　
* `"hello"`: 文字列
    * `'hello'`とシングルクォートで囲ってもよい <br />　
* `print`関数
    * 字を端末に出力する
    * C言語の`puts`や`printf`などに相当
    * 文字列も数字も（他のいくつかの型のものも）受け付け

---

## <span style="text-transform:none">hello.py</span>のコードの動き方

* コマンド「`python3`」がファイル「`hello.py`」の中身を見て実行
```bash
$ python3 hello
```
    * テキストファイルを読み込んで直接実行: <span style="color:red">インタプリタ</span>
        * 厳密には少し違うけど、とりあえずこの理解で
    * C言語と異なる
    ```bash
    $ gcc hoge.c    #gccでhoge.cをコンパイル
    $ ./a.out       #できたa.outを実行
    ```

---

## 問題: 次のような需要が存在

* `./a.out`みたいに、次のように実行したくないか
```bash
$ ./hello.py
```
    * なぜそんな需要があるのかは後述<br />　
* 解決法
    * <span style="color:red">shebang</span>（「シバン」と呼ぶ）を使用
    * 次のように`hello.py`を変更（1行目がシバン）
        ```python
        #!/usr/bin/python3
                                #この空行はいらないが、読みやすさのためにあける
        print("hello")
        print(3.14)
        ```
        * 注意1: インタプリタはパスで指定（<span style="color:red">`which python3`</span>で調査可能）
        * 注意2: 1行目にはコメントを入れないこと
        * 注意3: `#!`の前に空白を入れないこと

---

## <span style="text-transform:none">hello.py</span>の単独実行

* shebangの他、`hello.py`の<span style="color:red">実行</span>を許可する手続きが必要
    * 文章かもしれないファイルを実行したら事故が起こるので制限されている
    * `ls -l`で出てくる1列目のデータに「`x`」がないと実行できない
        ```bash
	$ ./hello.py                            #実行してみる
        bash: ./hello.py: 許可がありません      #できない
        $ ls -l hello.py                        #ls -lで確認
        -rw-r--r-- 1 ueda ueda 47 11月 30 06:50 hello.py
        $ ls -l a.out                           #gccで作ったa.outには最初から存在
        -rwxr-xr-x 1 ueda ueda 16464 11月 30 07:16 a.out
        ```
    * 手続き: <span style="color:red">`chmod`</span>を使用
        ```bash
	$ chmod +x hello.py
        $ ./hello.py                            #実行できる
        hello
        3.14
        $ ls -l hello.py
        -rwxrwxr-x 1 ueda ueda 144 11月 30 18:31 hello.py
        ```


---

## パーミッション

* `ls -l`で1列目に出てくるファイルの利用条件
    * そのファイルの「読む権利」「書く権利」「実行する権利」
        ```bash
        $ ls -l hello.py
        -rwxr-xr-x 1 ueda ueda 47 11月 30 06:50 hello.py
        ```
* 当面は最初の`rwx`を気にするとよい 
    * `r`が存在: 読める
    * `w`が存在: 書ける
    * `x`が存在: 実行できる


---

## さらなる問題: `./hello.py`の<br />`./`を取りたい

* なぜ`hello.py`には、`./`が必要なのか
    * `ls`などのコマンドも`./hello.py`もプログラム。
    * shebangでは`/usr/bin/python3`と、パスでインタプリタを呼び出した
    * `ls`もパスで呼び出せる
    

    
    違いは？<br />　
* 種明かし
    * シェルが探している

---

## まとめ

* プログラムを書いて動かした
* プログラム周りのLinuxの機能を確認

* 重要語句
    *

