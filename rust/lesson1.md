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
     7     int &hoge_ref = hoge[1];
     8     std::cout << hoge_ref << std::endl;
     9     delete [] hoge;
    10     std::cout << hoge_ref << std::endl; //変なところを参照
    11 }
    ```
* 対策
    * Java, Python, Ruby, ...: garbage collector, GC（実行時に確認） 
    * <span style="color:red">Rust: bollow checker（コンパイル時に確認）</span>
        * 実行時はC/C++と同じようにGCなし（原理的には同等に速い。ロボット向き。）

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
    * データを使っていない
    * 変数、関数、引数を使っていない
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
    * ワーニングに対処することでコードが散らからなくなる

---

## 関数

* `fn 関数名(引数) -> 戻り値の型`で定義
    ```rust
    1 fn chan(s: String) -> String { //s: 引数、String: 文字列型
    2    s + "-chan" // return s + "-chan"; でもよい
    3 }
    4
    5 fn main() {
    6     let x = "neko".to_string(); //xを文字列型として定義
    7     let y = chan(x);            //xをchanに渡す
    8     println!("{}", y);          //yをプリント
    9 }
    ```
    * （まだわからなくていいけど）`println!`は関数ではなくマクロ。様々な型を受け入れ可能

---

## 所有権

* 前のページのコードに次のように追記するとエラーが発生
    ```rust
    5 fn main() {
    6     let x = "neko".to_string(); //xを文字列型として定義
    7     let y = chan(x);            //xをchanに渡す
    8     println!("{}", y);          //yをプリント
    8     println!("{}", x);          //xをプリント
    9 }
    ```
    * エラーの内容（<span style="color:red">move</span>がどうたらと出てくる）
    ```rust
    $ cargo build
    error[E0382]: borrow of moved value: `x`
     --> src/main.rs:9:20
    　|
    6 |     let x = "neko".to_string();
    　|         - move occurs because `x` has type `String`, which does not implement the `Copy` trait
    7 |     let y = chan(x);
    　|                  - value moved here
    8 |     println!("{}", y);
    9 |     println!("{}", x);
    　|                    ^ value borrowed here after move
    ```

---

## 所有権（続き）

* 基本的に、変数の値（=名前のついたデータ）は<span style="color:red">関数や`=`で別の変数に渡すとデータが移ってしまう</span>
    * 変数（名前）のほうは何もデータを指さなくなる
    * <span style="font-size:50%">数値など簡単な型のデータ以外は</span>明示的に指示しないとデータが複製されない
        * C/C++だと大きなデータでも引数に渡すとコピーされるので挙動が違う
        * Pythonみたいに参照だらけにはならない
    * 「所有権」という考え方
        * 「move」とは所有権の移動


---

## 参照と借用

* `&`をつけると参照になる
    * 参照: この場合はデータの覗き窓と考えるとよい
    * 下のコードでは`x`の所有権は失われていない
        * 関数が一時的に「借用」した状態になる
    ```rust
     1 fn chan(s: &String) -> String { //引数を参照に
     2    s.clone() + "-chan" //clone: 参照から値をコピー
     3 }
     4 
     5 fn main() {
     6     let x = "neko".to_string();
     7     let y = chan(&x); //参照として渡す
     8     println!("{}", y);
     9     println!("{}", x);
    10 } //このコードではエラーもワーニングも出ない
    ```

---

## 参照を利用した変数の書き換え

* `&mut`で引数をとると、呼び出し側の元の変数を書き換え可能
    ```rust
    1 fn chan(s: &mut String) {
    2     s.push_str("-chan");
    3 }
    4 
    5 fn main() {
    6     let mut x = "neko".to_string();
    7     chan(&mut x); //可変な変数の参照を渡す
    8     println!("{}", x);
    9 }
    ```

---

## 参照と借用

* 貸しているものは渡せない
* これをコンパイルするとどうなる？
    ```rust
    1 fn main() {
    2     let x = "neko".to_string();
    3     let y = &x; //移動していないが借用している
    4     let z = x;  //xからデータがなくなる（yはどうなる？）
    5     println!("{}", y);
    6 }
    ```
    * 「borrowされているからmoveできない」という出力
    ```rust
    $ cargo run
    （略）
    error[E0505]: cannot move out of `x` because it is borrowed
     --> src/main.rs:4:13
    　|
    3 |     let y = &x;
    　|             -- borrow of `x` occurs here
    4 |     let z = x;
    　|             ^ move out of `x` occurs here
    5 |     println!("{}", y);
    　|                    - borrow later used here
    ```

---

## ベクタ型

* `Vec<任意の型>`でベクタが定義可能
    * ベクタ: 配列（のようなもの）
    * C++のvector型
    ```rust
    1 fn main() {
    2     let v = vec![1, 2, 3]; //vec!マクロで初期化
    3     let w :Vec<i32> = vec![4, 5, 6]; //型を明示
    4     println!("{:?}", &v);
    5     println!("{:?}", &w);
    6 }
    ```
    * 可変なベクタ
    ```rust
    1 fn main() {
    2     let mut v = vec![1, 2, 3];
    3     v.insert(0, 10000);
    4     v.pop();
    5     println!("{:?}", &v); 
    6 }   // ↑ [10000, 1, 2]と出力
    ```


---

## 列挙型

* C/C++のenumと異なり、データも持たせることができる
    ```rust
     1 #[derive(Debug)] // {:?}でプリントできるように書く（アトリビュート）
     2 enum NumOrStr {
     3     Num(i32),
     4     Str(String),
     5     Hoge,
     6 }
     7
     8 fn main() {
     9     // x, y, zは同じ型
    10     let x = NumOrStr::Num(10);
    11     let y = NumOrStr::Str("waowao".to_string());
    12     let z = NumOrStr::Hoge;
    13
    14     println!("x: {:?}, y: {:?}, z: {:?}", x, y, z);
    15 }
    ```
    * 実行
        ```bash
        $ cargo run
        x: Num(10), y: Str("waowao"), z: Hoge
        ```


---

## 列挙型と<span style="text-transform:none">match</span>による場合分け


---

## <span style="text-transform:none">Option</span>型

* 値があるかどうか分からないもので利用される
    ```rust
    1 use std::env;
    2
    3 fn main() {
    4     let arg = env::args().nth(1);
    5     println!("{:?}", &arg);
    6 }
    ```
    * 実行
        ```bash
        $ ../target/debug/hello
        None         #ない
        $ ../target/debug/hello aaa
        Some("aaa")  # むき出しの"aaa"ではなく、Some()にくるまれて返ってくる
        ```
   * C/C++のNULLやPythonのNoneなどと異なり型のレベルで「ない」を表現


---

## <span style="text-transform:none">match</span>による場合分け

```rust
1 use std::env;
2
3 fn main() {
4     let arg = env::args().nth(1);
5     match arg {
6         Some(a) => println!("引数は「{}」", &a),
7         None    =>  println!("引数ないです"),
8     }
9 }
```

