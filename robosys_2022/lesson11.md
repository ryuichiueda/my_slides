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

ひとつのオブジェクトで関連する属性を<br />まとめて管理可能

---

### 2. オブジェクトの作り方

* クラスを使う
  * 例: `node = Node("talker")`
    * 「`Node`というのはこういう属性を持つよ」とどこかに定義されている
      * この定義を書いたものがクラス
      * どこかに`class Node...`と書いたスクリプトがある。<br />（余力のある人は`grep`とかで探してみましょう）

---

## 3. クラスの作成

* `talker.py`をクラスを使った記述に変更

