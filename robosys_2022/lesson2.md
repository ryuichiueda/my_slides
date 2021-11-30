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
        ```python
        $ python3 hello.py
        hello
        3.14
        ```

---

## <span style="text-transform:none">hello.py</span>のコード

* `print`関数
    * 字を端末に出力する
    * C言語の`puts`や`printf`などに相当
