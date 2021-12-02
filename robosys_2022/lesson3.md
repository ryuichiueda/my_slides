# ロボットシステム学

## 第3回: <span style="text-transform:none">Linux環境での<br />PythonプログラミングII</span>

千葉工業大学 上田 隆一

<br />

<p style="font-size:50%">
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">
<img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a>
</p>

---

## 今日やること

* 前半: Pythonで引数を操作
* 後半: 標準入出力を操作

---

## 前半: 引数の処理

---

## <span style="text-transform:none">Python</span>での引数の処理

* 要点
    * システムのことを扱う<span style="color:red">sysモジュール</span>を読み込む
        * 他によく使うモジュールとしては<span style="color:red">math</span>など
    * 引数はリストになっている
* 例（`args`）
    ```python
    #!/usr/bin/python3
    import sys             #sysモジュールを読み込み
    
    print(sys.argv)        #sysの「下の」argvに引数が入るのでprintしてみる
    ```
* 実行（「`args`」という名前でコードを保存して実行）
    ```bash
    $ ./args 引数1 引数2 引数3
    ['./args', '引数1', '引数2', '引数3']    #端末に打った通りにリストに入る
    ```

---

## 引数を使ったコマンドの作成1

* 引数の数を足し合わせる`plus`というコマンドを作ってみましょう
    * とりあえず2つの引数を足す計算のコードを作る
    * 例（`plus_a`）
        ```python
        #!/usr/bin/python3
        import sys
        
        print( float(sys.argv[1]) + float(sys.argv[2]) )
        ```
        * <span style="color:red">`float`</span>: 文字列を浮動小数点数に変換 
    * 実行
        ```bash
        $ ./plus_a 1 2
        3.0
        ```

---

## 引数を使ったコマンドの作成2

* 今度は引数をすべて足すコマンドを作成
    * 例（`plus_b`）
        ```python
        #!/usr/bin/python3
        import sys
        
        x = 0.0                    #xを定義して初期化
        for n in sys.argv[1:]:     #スライスを使う
            x += float(n)          #+=で足し込む
        
        print(x)
        ```
        * 一気にコードを書かず、引数がちゃんと読み取れているか`print`で確認しながら書くこと
    * 実行
        ```bash
        $ ./plus_b 1 2 3 4 5 6 7 8 9 10
        55.0
        ```

---

## 引数を使ったコマンドの作成3

* Pythonの「リスト内包表記」を利用
    * 発展的な内容なのでPython初心者の人はやらないでよいです
    * 例（`plus_c`）
        ```python
        #!/usr/bin/python3
        import sys
        
        nums = [ float(e) for e in sys.argv[1:] ]   #引数を浮動小数点数に変換してリスト作成
        print(sum(nums))                            #リストの和を返すsum関数を利用
        ```
        * 実行例は省略


---

## 後半: 標準入出力

---

## コマンドの出力のファイルへの保存

* 端末への出力は<span style="color:red">「 > ファイル」</span>でファイルに保存可能
    ```python
    $ ./plus_c 1 2 3 4 5 6 7 8 9 10 > ans    #ansというファイルに出力を保存
    $ cat ans                                #ansの中身の確認
    55.0
    ```
    * （出力の）<span style="color:red">リダイレクト</span>と呼ばれる機能<br />　
* リダイレクト
    * シェルが機能を提供
    * コマンド（プログラム）は、とりあえず端末に字を出すようにしておけば、処理結果の保存先を気にしなくてよい
        * コマンドにおける端末に字を出す出口: <span style="color:red">標準出力</span>と呼ばれる
        * プログラムを書くときは、特に理由がなければ標準出力に結果を出す

---

## ファイルからの入力

* ファイルの中身は<span style="color:red">「 &lt; ファイル」</span>でコマンドに渡せる
* Pythonでの受け取り方
    * <span style="color:red">`sys.stdin`</span>とfor文で1行ずつ受け取る
    * コードを書く前の準備（ファイルの作成）
        ```bash
        $ seq 10 > nums    #リダイレクトをつかってnumsというファイルを作成
        $ cat nums
        1
        2
        	・・・
        10
        ```
        * <span style="color:red">`seq`</span>: 整数を順に出力するコマンド
    * コード（`read_stdin`）
    ```python
    #!/usr/bin/python3
    import sys                 #余白の関係で詰めてますが下に1行空行があったほうがよいです
    for line in sys.stdin:
        print(line)
    ```


$ ./read_stdin < ans
1

2

3

4

5

6

7

8

9

10
