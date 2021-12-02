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

* Pythonで引数、標準入出力を操作

---

## <span style="text-transform:none">Python</span>での引数の処理

* 要点
    * システムのことを扱う<span style="color:red">モジュール</span>を読み込む
    * 引数はリストになっている
* 例
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

## 引数を使ったコマンドの作成

* 引数の数を足し合わせる`plus`というコマンドを作ってみましょう
    * とりあえず2つの引数を足す計算のコードを作る
    * 例
        ```python
        #!/usr/bin/python3
        import sys
        
        print( int(sys.argv[1]) + int(sys.argv[2]) )
        ```
