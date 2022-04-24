# ロボットシステム学

## 第2回: <span style="text-transform:none">Linux環境での<br />PythonプログラミングI</span>

千葉工業大学 上田 隆一

<br />

<p style="font-size:50%">
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">
<img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a>
</p>

---

## 今日やること

* 前半: Pythonの入門
    * C言語をちょっとやったくらいの人を想定
    * 変数、関数、リスト、for文
        * とりあえずこれくらい覚えておけば、できることは結構多い<br />　
* 後半: LinuxでPython等のコードを使うときの方法
    * こっちの方が大変
    * 「ロボットシステム学」なので

---

## 前半: <span style="text-transform:none">Python入門</span>

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

## <span style="text-transform:none">Python</span>の文字列、数字と関数

* 文字列、数字（<span style="color:red">リテラル</span>）
    * 文字列は`""`や`''`で囲む
        * `"hello"`、`'hello'`、"That's a pen."
    * 数字はそのまま記述
        * `3.14`、`-1`、`5`
        * `1.23e+100`（1.23かける10の100乗）<br />　
* 関数の使用
    * 「`名前(引数, 引数, ...)`」と記述
    * `print`関数
        * 字を端末に出力する
        * C言語の`puts`や`printf`などに相当
        * 文字列も数字も（他のいくつかの型のものも）受け付け


---

## <span style="text-transform:none">Python</span>の変数

* 要点
    * 値（数字や文字列、その他のもの）に名前を付与できる
        * 「値を変数に代入」というより「値に名前をつける」
    * 型は書かなくてよい
* コードの例
    ```python
    name = "上田"   #「上田」という文字列にnameという名前を付ける
    money = 5       #5という数字にmoneyという名前をつける
    print(name + "の所持金: " + str(money) + "円") #文字列をくっつけて出力
    print("{}の所持金: {}円".format(name, money) ) #発展: こういう書き方も
    ```
    * <span style="color:red">`str`</span>は数字などを文字列に変換する関数
    * 実行（`var.py`というファイル名で）
```bash
$ python3 var.py
上田の所持金: 5円
上田の所持金: 5円
```

---

## <span style="text-transform:none">Python</span>のリストと<span style="text-transform:none">for</span>文

* 要点
   * Pythonのリスト: 複数の値を持っておけるもの
   * Pythonのfor文: リストの要素をひとつずつ処理するもの
       * for文の中身は右側に余白（<span style="color:red">インデント</span>）
* コードの例　　　　　　　　　　　　　　　　　　　　　　　
    ```python
    fruits = ["apple", "banana", "chery" ]  #文字列3つのリストをfruitsと命名
    
    for f in fruits:
        print(f + "はおいしい")             #インデントは半角空白4文字が標準
    ```
    * 実行（`fruits.py`というファイル名でコードを保存）
        ```python
        $ python3 fruits.py
        appleはおいしい
        bananaはおいしい
        cheryはおいしい
        ```
---

## リスト要素の取得

* 要点
    * 基本はC言語と同じ
    * 負の値を指定すると後ろから要素を指定可能
* コードの例（`list.py`）
    ```python
    fruits = ["apple", "banana", "chery" ]
    print("最初の要素: " + fruits[0])      #先頭は0番目
    print("次の要素: " + fruits[1])        #次の要素は1番目
    print("最後の要素: " + fruits[-1])     #最後の要素
    ```
* 実行結果
    ```bash
    $ python3 list.py
    最初の要素: apple
    次の要素: banana
    最後の要素: chery
    ```

---

## リスト要素の範囲指定<br />（スライス）

* リストを部分的に取り出して新たにリストを作成できる
* コードの例（`list2.py`）
    ```python
    fruits = ["a", "b", "c", "d", "e" ]
    print("0番目から2番目の要素: ", fruits[0:3])
    print("2番目以降の要素: ", fruits[2:])
    print("ひとつ飛ばしでリストを作成: ", fruits[0::2])
    ```
    * 注意: 文字列とリストは`+`で連結できないので、上のprintでは引数を分けている
* 実行結果
    ```bash
    $ python3 list2.py 
    0番目から2番目の要素:  ['a', 'b', 'c']
    2番目以降の要素:  ['c', 'd', 'e']
    ひとつ飛ばしでリストを作成:  ['a', 'c', 'e']
    ```

---

## 練習

* 次のようなPythonのスクリプトを作ってみましょう
    * なにかリストを作成して、作ったリストについて、奇数の要素だけ出力（`print`）
    * さらに余裕があれば、出力する際になにか文字を付加
        * 例: 6ページ or 7ページ

---

## 後半: <span style="text-transform:none">Linux</span>とスクリプト

---

## <span style="text-transform:none">hello.py</span>のコードの動き方

* コマンド「`python3`」がファイル「`hello.py`」の中身を見て実行
```bash
$ python3 hello.py
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

* `./a.out`みたいに、次のように実行したい
```bash
$ ./hello.py
```
    * プログラムを使う側は、インタプリタが何かを意識したくない
    * 一応「`.py`」と拡張子があるが・・・
        * Python 2.xかもしれない
        * 本来いらないはず（`ls`が`ls.py`という名前だとおかしい）<br />　
* 可能とするには2つ作業が必要
    * シバンによるインタプリタの指定
    * 実行権限の付与

---

## 作業1: インタプリタの指定

* <span style="color:red">shebang</span>（「シバン」と呼ぶ）を使用
    * 次のように`hello.py`を変更（1行目がシバン）
        ```python
        #!/usr/bin/python3
                          #この空行はいらないが、読みやすさのためにあける
        print("hello")
        print(3.14)
        ```
    * シバンの書き方
        * 1行目に「`#!インタプリタ`」と記述
            * インタプリタはパスで指定（<span style="color:red">`which python3`</span>で調査可能）
            * インタプリタも含め、<span style="color:red">コマンドはファイル</span>
        * `#!`の前に空白を入れない
    * Linuxがシバンを読んでインタプリタを起動

---

## 作業2: 実行権限の付与

* `ls -l`で出る表示の1列目に「`x`」がないと実行できない
    * <span style="color:red">パーミッション</span>（次ページ）
    * 事故防止のため
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

## <span style="text-transform:none">hello（.py）</span>の実行

* こうなればOK
    ```bash
    $ ./hello.py
    hello
    3.14
    ### 拡張子を除去してもよい（拡張子をつけるかどうかは場合による） ###
    $ mv hello.py hello
    $ ./hello
    （略）
    ```
    * <span style="color:red">`mv`</span>: 移動、名前の変更<br />　
* `PATH`を使ってみる（前回参照）
    * 「パスの通った」コマンドとして呼び出せる
        ```bash
        $ PATH=$PATH:/home/ueda  #PATHの値に「:今いるディレクトリ」と文字列を追加
        $ hello
        （略）
        $ exit                   #シェルを終了すると追加した文字列は消える
        ```

---

## 練習

* なにかプログラムを作り、シバン、パスを駆使して、<br />パスの通ったコマンドとして実行

---

## まとめ

* Pythonのプログラムを書いて動かした
* Pythonのプログラムの裏でのLinuxやシェルの挙動を確認
    * シバン
    * パーミッション<br />　
* 重要語句: インタプリタ、シバン、パーミッション、「パスが通った」
* コマンド: `python3`、`which`、`chmod`、`mv`

