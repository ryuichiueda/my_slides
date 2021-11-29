# ロボットシステム学

## 第2回: Linux環境の使い方

千葉工業大学 上田 隆一

<br />

<p style="font-size:50%">
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">
<img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a>
</p>

---

## 今日やること

* Linuxの基本的な仕組みを理解する
* CLIに慣れる

---

## 端末 

* WSL（Windows）、Terminal（Mac, Linux）を立ち上げると字を打ち込める画面が出る
    * 「端末（terminal）」というもの

![](figs/terminal.png)

---

## 端末でなにをするか？

* ほとんどの場合、「プログラムを呼び出す」ために使用
    * 例: `ls`（ファイルのリスト表示プログラム）の呼び出し
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
