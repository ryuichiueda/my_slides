# ロボットシステム学

## 第2回: <span style="text-transform:none">Linux環境の使い方とPythonプログラミングI</span>

千葉工業大学 上田 隆一

<br />

<p style="font-size:50%">
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">
<img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a>
</p>

---

## 今日やること

* CLIに慣れる
* Pythonでプログラムを動かす
    * 文法については次回
* Linuxの基本的な仕組みを理解する

---

## 端末 

* WSL（Windows）、Terminal（Mac, Linux）を立ち上げると字を打ち込める画面が出る
    * <span style="color:red">「端末（terminal）」</span>というもの

![](figs/terminal.png)

---

## 端末でなにをするか？

* ほとんどの場合、「プログラムを呼び出す」ために使用
    * 例: <span style="color:red">`ls`</span>（ファイルのリスト表示プログラム）の呼び出し
```bash
WSL$ ls /mnt/c/Windows/
Ubuntu$ ls /etc/
```
    * GUIのアプリも立ち上げられる
```bash
### エクスプローラーを立ち上げる（上田の経験ではGUIより反応が良い）###
WSL$ explorer.exe
WSL$ explorer.exe .
### Ubuntuで特定のフォルダを開く###
Ubuntu$ nautilus /etc/
```

普段から使うようにしてみましょう。

---

## プログラムを書く

* `ls`のように端末から呼び出すプログラムを書いて実行
    * 手順
        1. <span style="color:red">`nano hello.py`</span>と端末に打ってエディタを立ち上げ
            * エディタ: プログラムや文章を書くプログラム
        2. プログラムを書いて保存<br />
<img width=50% src="figs/nano.png" /><br />
        3. 下の`^O`（Ctrl+O)で保存
            * `File Name to Write: hello.py`と聞かれるのでEnter
        4. `^X`（Ctrl+X）で終了

---

## 書けているか確認

* `ls`で<span style="color:red">ファイル</span>ができているか確認
* <span style="color:red">`cat`</span>で、書いた内容を確認

```bash
$ ls hello.py
hello.py
$ cat hello.py
#!/usr/bin/python3

print("hello")
```


---

## 実行できるようにする

* ファイルを作っただけではプログラムとして実行不可能
    * 普通の文章をプログラムとして実行しないため
    * <span style="color:red">`chmod`</span>で実行可能に

```bash
$ ls -l hello.py
-rw-r--r-- 1 ueda ueda 32 11? 29 16:33 hello.py
$ chmod +x hello.py
```



---

## まとめ

* 単にhelloと表示するプログラムを書いただけだが・・・
    * いろいろな仕掛けがあって動いていることを確認
        * GUIを使っていると仕掛けが隠れていて、なんとなくプログラムが書けるようになっているが、機械を扱う我々には不十分
    * 重要語句: 端末、ファイル<br />　
* 普段から使わないと自由自在にあやつるには時間がかかるので、普段から使ってみましょう。
    * ファイルを探す、開く
