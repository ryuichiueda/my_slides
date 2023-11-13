# <span style="text-transform:none">Rust</span>入門

## 第1回（1回だけ）

千葉工業大学 上田 隆一

<br />

<p style="font-size:50%">
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">
<img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a>
</p>

---

## 今日やること

* Rustについて
* なんでRust？
* 触ってみる

---

## <span style="text-transform:none">Rust</span>について

<span style="font-size:35%">出典: Clive Thompson: [How Rust went from a side project to the world’s most-loved programming language](https://www.technologyreview.com/2023/02/14/1067869/rust-worlds-fastest-growing-programming-language/), MIT Technology Review, 2023.</span>

* 2006年、MozillaのエンジニアGraydon Hoareの個人プロジェクトとしてスタート
    * 「C/C++で書かれたもののバグが多すぎる（メモリまわり）」
    * 他のガベージコレクタを有する言語は遅い<br/>　
* <span style="font-size:35%">（いろいろあったと思うけど全部すっ飛ばすと）</span>→独自のメモリ管理方法で安全性と軽さを両立する言語となる

---

## 「メモリ管理」

* C++やCでよくある話
    * 参照の先がなくなっている
    ```cpp
     1 #include <iostream>
     2
     3 int main() {
     4     int *hoge = new int[2];
     5     hoge[0] = 1;
     6     hoge[1] = 2;
     7
     8     int &hoge_ref = hoge[1];
     9
    10     std::cout << hoge_ref << std::endl;
    11     delete [] hoge;
    12     std::cout << hoge_ref << std::endl; //変なところを参照
    13 }
    ```
* 対策
    * Java, Python, Ruby, ...: ガベージコレクタ（実行時にうまくやる） 
    * <span style="color:red">Rust: 言語の仕組みで防止+コンパイル時に全部チェック</span>
        * 実行時はC/C++と同じようにガベージコレクタなし（原理的には同等に速い。ロボット向き。）

---

## 早速使ってみる1

* 環境のセットアップ
    ```bash
    $ sudo apt install cargo
    ```
    * `cargo`: Rustのビルドシステム兼パッケージマネージャ<br />　
* プロジェクトを作る
    ```bash
    $ cargo new hello
     Created binary (application) `hello` package
    $ cd hello
    $ ls -l
    合計 8
    -rw-rw-r-- 1 ueda ueda  174 11月 13 10:01 Cargo.toml #設定ファイル
    drwxrwxr-x 2 ueda ueda 4096 11月 13 10:01 src #この中にソースを置く
    ```

---

## 早速使ってみる2

* コードがもう書いてある
    ```bash
    $ cd src
    $ cat main.rs
    fn main() {
        println!("Hello, world!");
    }
    ```
* コンパイル/ビルド/実行してみる
    ```bash
    $ cargo build
       Compiling hello v0.1.0 (/home/ueda/tmp/hello)
        Finished dev [unoptimized + debuginfo] target(s) in 0.23s
    Hello, world! # 出力
    ```

---

## 変数

* `let`で定義
   ```rust
   1 fn main() {
   2     let x = 1;
   3     println!("{}", x);
   4 }
   ```
* 基本、変数は（変数なのに）定数
   ```rust
   1 fn main() {
   2     let x = 1;
   3     x = 2;    //cargo buildするとエラーが出る
   4     println!("{}", x);
   5 }
   ```
   * 変えたければ`mut`（ミュータブル）をつける
   ```rust
   1 fn main() {
   2     let mut x = 1;
   3     x = 2; 
   4     println!("{}", x);
   5 } //ただし、「1」を使ってないぞとワーニング
   ```

---

## 余談: コンパイラがやたら丁寧

* C/C++のものよりも細かくワーニングが出る
    * ワーニングには対処を
    ```rust
    $ cargo build
    （いろいろ省略）
     --> src/main.rs:2:13
    　|
    2 |     let mut x = 1;
    　|             ^
    　|  
      = help: maybe it is overwritten before being read?
    ```
    * 他に出るワーニング（ありがたい）
        * 変数、関数、引数を使っていない
        * 他
* 変数の使用状況の他、メモリが安全に使われているかまでコンパイラが見ている
    * より正確にはbollow checkerが見ている

---
