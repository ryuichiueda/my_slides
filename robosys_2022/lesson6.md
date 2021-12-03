# ロボットシステム学

## 第6回: 

千葉工業大学 上田 隆一

<br />

<p style="font-size:50%">
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">
<img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a>
</p>

---

## 標準エラー出力

* コマンドにはパイプやリダイレクトで渡したくない出力も存在
    * エラーの際のメッセージなど
    * 例: `plus_stdin`に文字を入力してみる
    ```bash
    $ echo あ | ./plus_stdin 
    Traceback (most recent call last):
      File "./plus_stdin", line 6, in <module>
        ans += float(line)
    ValueError: could not convert string to float: 'あ\n'
    $ echo あ | ./plus_stdin > ans
    ### もしこれでエラーがansに入ったらエラーに気づかない ###
    ```
        * 実際はちゃんと画面にエラーが出てくる<br />　
* ちゃんとしたコマンドはエラーを<span style="color:red">標準エラー出力</span>に出す
    * 標準出力とは別の出口

---

## <span style="text-transform:none">Python</span>でのエラーの表示

* 要点
    * なにもしなくてもエラーのメッセージは出てくる
        * 読んでますか？
    * エラーは<span style="color:red">`try`</span>で捕まえ、<span style="color:red">`except`</span>で対処
    * 標準エラー出力には、「ファイル」<span style="color:red">`sys.stderr`</span>に出力
* 例（`plus_err`）
    ```python
    #!/usr/bin/python3
    import sys               #注意1: 空行を詰めてます
    ans = 0.0                #注意2: このコードには少し問題がある
    for line in sys.stdin:
        try:
            ans += float(line)
        except:                          #stderr: standard errorのこと
            print("数字への変換失敗（不正な入力）", file=sys.stderr)
    print(ans)
    ```
    * 実行例は下に

>>>

* 正常な入力の場合
    ```bash
    $ echo 1 2 3 | tr ' ' '\n' | ./plus_err
    6.0
    ```
* 数字ではない入力の場合　　　　　　　　　　　
    ```bash
    $ echo あ | ./plus_err 
    数字への変換失敗（不正な入力）
    0.0
    $ echo あ | ./plus_err > ans    #標準出力をファイルへ
    数字への変換失敗（不正な入力）  #標準エラー出力はファイルに入らない
    $ echo あ | ./plus_err 2> error  #標準エラー出力をファイルに書き出し
    0.0
    $ echo あ | ./plus_err &> error  #出力すべてをファイルに書き出し
    ```
    * <span style="color:red">`2>`</span>: 標準エラー出力のリダイレクト
    * <span style="color:red">`&>`</span>: 標準出力とエラー出力のリダイレクト


---

## エラーによる終了と<br />終了ステータス

