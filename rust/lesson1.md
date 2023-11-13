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

---
