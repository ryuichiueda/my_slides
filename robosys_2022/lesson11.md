# ロボットシステム学

## 第11回: <span style="text-transform:none">Python</span>の<br />クラスとオブジェクト

千葉工業大学 上田 隆一

<br />

<p style="font-size:50%">
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">
<img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a>
</p>

---

## 今日やること

* 作ってきたROS2のパッケージのコードをクラスで整理
  1. オブジェクトとクラスの説明
  1. 実践
  1. まとめ

---

## 1. オブジェクトとクラス

---

### <span style="text-transform:none">Python</span>のオブジェクト

* ドットをつけると持っている変数や関数が使える
  * 例: 
    * `msg.data = n`
    * `pub.publish(msg)`
  * 注意: オブジェクトの持っている変数や関数は、<br />正確には「属性」（attribute）と呼ばれる<br />　
* ひとつのオブジェクトで関連する属性を<br />まとめて管理可能
  * ROS2のノードと同様、機能をうまく切り分けていくと<br />分かりやすいソフトウェアに

---

### 2. オブジェクトの作り方

* クラスを使ってオブジェクトを作る
  * 例: `node = Node("talker")`
    * 「`Node`」という<span style="color:red">クラス</span>にオブジェクトの定義が書いてあって、<br />それにしたがって`node`オブジェクトができる
    * C言語の「`struct 構造体の名前 変数の名前;`」に相当
    * カッコの中の文字列`"talker"`は、オブジェクトの初期化のときに<br />渡す引数<br />　
  * 遺言
    * うまくクラスを使えるようになるには豊富な経験が必要
    * 人のコードを使うときに、どういう属性のまとめ方をしているか<br />観察を

---

## 3. クラスの作成

* `talker.py`をクラスを使った記述に変更してみましょう

---

### クラスの作成と初期化（その1）

* やること（その1）
  * 1. `Talker`というクラスを作成
  * 2. `Talker`にパブリッシャーと変数を属性として実装

```python
  1 import rclpy
  2 from rclpy.node import Node
  3 from std_msgs.msg import Int16
  4
  5 class Talker():          #ヘッダの下にTalkerというクラスを作成
  6     def __init__(self):  # オブジェクトを作ると呼ばれる関数
  7         self.pub = node.create_publisher(Int16, "countup", 10)
  8         self.n = 0
  9         # ↑ selfはオブジェクトのこと
            # ↑ オブジェクトにひとつパブリッシャと変数をもたせる
（次ページに続く）
```

---

### クラスの作成と初期化（その2）

* やること（その2）
  * 3. オブジェクトの作成
  * 4. コールバック関数の書き換え
    * オブジェクトのパブリッシャや変数を使うように
* 書き換えたら動作確認を

```python
 10 rclpy.init()
 11 node = Node("talker")
 12 talker = Talker()      #オブジェクトを作成（__init__が実行される。）
 13
 14 def cb():              #関数内のnやpubをtalkerのものに変更
 15     msg = Int16()
 16     msg.data = talker.n
 17     talker.pub.publish(msg)
 18     talker.n += 1
 19
 20 node.create_timer(0.5, cb)
 21 rclpy.spin(node)
```

---

### メソッドの追加

* `cb`関数も`Talker`の<span style="color:red">メソッド</span>にしてしまいましょう
  * メソッド: オブジェクト内の変数などを操作するための<br />関数（のようなもの）

