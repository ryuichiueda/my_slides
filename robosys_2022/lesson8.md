# ロボットシステム学

## 第8回: <span style="text-transform:none">Robot Operating System (ROS2)</span>

千葉工業大学 上田 隆一

<br />

<p style="font-size:50%">
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">
<img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a>
</p>

---

## 今日やること

1. ROSの概要の理解
2. ROS2のインストール
3. ROSのノードと通信の基礎
4. ROS2プログラミング
5. まとめ
* 参考図書
    * 近藤 豊: [ROS2ではじめよう 次世代ロボットプログラミング](https://gihyo.jp/book/2019/978-4-297-10742-0), 技術評論社, 2019. 

---

## 1. ROSの概要

---

### <span style="text-transform:none">ROS: robot operating system

* ロボットのソフトウェアコンポーネントを作って動作させるためのフレームワーク/ミドルウェア
  * OSでは無い
* 発祥: 2000年代後半、Willow Garage社
* BSDライセンス
* サイト
  * 公式ページ: https://www.ros.org/
  * マニュアル等: https://docs.ros.org/

---

### どんなものか

* 本体: プロセス間通信をつかさどる
  * プロセス同士をpublish-subscribeモデルや<br />client-serverモデルでつなぐ
  * 通信するデータに型<br />　
* 周辺
  * ビルドシステム、パッケージ管理、テストツール、・・・

<span style="color:red;font-size:70%">と書いてもよくわからんので使うメリットから</span>

---

### ROS化されている<br />重要なソフトウェア

* 移動ロボットの制御ソフトウェア
  * gmapping, Cartographer, ナビゲーションメタパッケージ
    * 地図生成（次のページにデモ）、位置推定、経路生成<br />　
* マニピュレータのソフトウェア
  * MoveIt!
    * 腕の動作計画 腕先の位置を入力→関節角を計算（逆運動学）<br />　
* 各種センサのインタフェース
  * すぐ使える
  * 以前は（特にLinuxでは）自分でシリアル通信のプログラムを書くなどの苦労があった

---

### ROSを使った移動ロボットの制御

* コントローラでロボットに移動経路を教え、<br />そのあとロボットが自律で経路を移動
  * 地図と経路を計算するCartographerを利用

<iframe width="560" height="315" src="https://www.youtube.com/embed/eVHkHOCsHns" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

* ポイント: Cartographerの部分は一切手を入れていない

---

### ROSを使った<br />マニピュレーションの様子

* https://twitter.com/i/status/1201399538541400064
  * 2年生有志作
  * 動きはMoveIt!が生成
    * 自分で計算しなくていい（教員苦笑い）

---

### ROSのバージョン

* ROS1（もともとROSと呼ばれていたもの）
  * オリジナルのROS
  * 主に研究のフレームワークとして発達
  * まだまだ使われる/研究室でも使う
    * 習得は2021年までの講義資料でどうぞ（https://youtu.be/PL85Pw_zQH0 ）<br />　
* ROS2（今回扱うもの）
  * ROSが普及して、当初想定していなかった利用場面が増加
    * セキュリティー、製品化、シビアな通信環境、・・・
  * アーキテクチャから作り直し
    * ROS1とは基本的に互換性なし

---

## 2. ROS2のインストール

---

### ROS1がすでにインストールされている場合

* 方法1: 別の環境を準備する
* 方法2:
  * `.bashrc`から関係する記述をコメントアウト
  * ターミナルを出る

```bash
$ vi ~/.bashrc
（略）
#source /opt/ros/noetic/setup.bash     ←ここから関係する設定をコメントアウト
#source ~/catkin_ws/devel/setup.bash
#export ROS_MASTER_URI=http://localhost:11311
#export ROS_HOSTNAME=localhost
```

---

### ROS2のインストール

* [インストールスクリプト](https://github.com/ryuichiueda/ros2_setup_scripts/blob/master/setup.bash)
    * 手順は次の通り
    * 余計なものを入れたくない場合は`setup.bash`の`ros-${ROS_VER}-desktop`を`ros-${ROS_VER}-ros-base`に変更して実行

```bash
$ git clone https://github.com/ryuichiueda/ros2_setup_scripts
$ cd ros2_setup_scripts
$ ./setup.bash
$ source ~/.bashrc
### 動作確認 ###
$ ros2
usage: ros2 [-h] Call `ros2 <command> -h` for more detailed usage. ...

ros2 is an extensible command-line tool for ROS 2.
（以下略）
```
* <span style="color:red">さらに`~/.bashrc`に`export ROS_LOCALHOST_ONLY=1`と記述</span>

---

### 動作確認

* ひとつプログラムを立ち上げ
  ```bash
  端末1$ ros2 run demo_nodes_py talker
  ・・・
  [INFO] [talker]: Publishing: "Hello World: 61"
  [INFO] [talker]: Publishing: "Hello World: 62"
  ・・・
  ```
  * 「"Hello World: 数字"を発行」と言っている
* もうひとつプログラムを立ち上げ
```bash
端末2$ ros2 run demo_nodes_py listener
・・・
[INFO] [listener]: I heard: [Hello World: 55]
[INFO] [listener]: I heard: [Hello World: 56]
・・・
```
  * 「"Hello World: 数字"を聞いた」と言っている

で、これは何をしているのか？

---

## 3. ROSのノードと通信の基礎

---

### ROSのノードの基本

* 基本1: プログラムのことを「ノードと呼ぶ」
* 基本2: ノードは互いに通信する<br />　
* これで何ができるか？
  * ロボットの各機能を別のプログラムとして実装できる
    * 例1: 画像処理のプログラムとナビゲーションのプログラムの連携
      * 画像処理のプログラムは独立したソフトウェアとして公開可能
    * 例2: レーザースキャナをA社からB社に交換
      * A社とB社が同じ出力をするノードを作っていると、交換が容易

---

### ROSの通信の基本

* GUIツールで構造を確認
    ```bash
    端末1$ ros2 run demo_nodes_py talker
    端末2$ ros2 run demo_nodes_py listener
    端末3$ ros2 run rqt_graph rqt_graph
    ```
    <img src="./figs/rqt_graph_s.png" width="50%" />
* ノード（楕円）がデータの流路（矢印）で連結
  * `/chatter`: 流路の名前で「<span style="color:red">トピック</span>」と呼ばれる
    * 流れるデータは「<span style="color:red">メッセージ</span>」
  * ノードから出る矢印: 「<span style="color:red">パブリッシャ</span>」
  * ノードに入る矢印: 「<span style="color:red">サブスクライバー</span>」
* ノードは複数のパブリッシャとサブスクライバーを持てる
* <span style="color:red">メッセージは型を持つ</span>

---

## 4. ROS2プログラミング

* 先ほど動かしたtalker、listenerに相当するノードを作成
  * 言語: Python
  * [参考](https://index.ros.org/doc/ros2/Tutorials/Developing-a-ROS-2-Package/#python-packages)
* いろいろ作法が細かいので、コードを書く以外のことの意図も確認
  * 誰が作ったパッケージも同じような構造にするため

---

### ワークスペースの作成

* ワークスペース: 作業場
* とりあえずディレクトリを作るだけ

```bash
$ mkdir -p ros2_ws/src
$ tree ros2_ws/
ros2_ws/
└── src
1 directory, 0 files
```

---

### 初期状態のパッケージを作る

* パッケージ: ROSのプログラムの配布単位
    * GitHubのリポジトリにほぼ相当
    * なかにいくつかのノードのプログラムを入れる
* `ros2 pkg create`を使う
  * 下の例: mypkgというパッケージを作成
      ```bash
      $ cd ~/ros2_ws/src/
      $ ros2 pkg create mypkg --build-type ament_python
      going to create a new package
      package name: mypkg
      ・・・
      $ tree
      .
      └── mypkg
          ├── mypkg
          │   └── __init__.py
          ├── package.xml
          ├── resource
          │   └── mypkg
          ├── setup.cfg
          ├── setup.py
          └── test
      （略）
      ```

---

### パッケージ情報の記述1

* `package.xml`を編集する
    * パッケージマニフェストというもの
    * パッケージの情報を記述したファイル
        * 説明（description）、メンテナ（maintainer、管理する人）、ライセンスをちゃんと書く

```xml
<?xml version="1.0"?>
<?xml-model href="http://download.ros.org/..."?>
<package format="3">
  <name>mypkg</name>
  <version>0.0.0</version>
  <description>a package for practice</description>
  <maintainer email="ryuichiueda@example.com">Ryuichi Ueda</maintainer>
  <license>BSD-3-Clause</license>

  <test_depend>ament_copyright</test_depend>
・・・
```

---

### パッケージ情報の記述2

* `setup.py`の編集
    * Pythonのモジュールで使われるインストールスクリプト
        * 変更箇所は`package.xml`と同じ

```
from setuptools import setup

package_name = 'mypkg'

setup(
    name=package_name,
    version='0.0.0',
    ・・・
    maintainer='Ryuichi Ueda',
    maintainer_email='ryuichiueda@gmail.com',
    description='a package for practice',
    license='BSD-3-Clause',
・・・
```

---

### パッケージをビルド

* ビルド作業
  * `colcon`というコマンドを使用
  ```bash
  $ cd ~/ros2_ws/
  $ colcon build
  （略。警告が出るけどとりあえず気にしない）
  Summary: 1 package finished [0.69s]
    1 package had stderr output: mypkg 
  ```
* `ros2_ws`下のパッケージを利用可能に
  * `~/.bashrc`の末尾に2行追加
    ```bash
    source ~/ros2_ws/install/setup.bash
    source ~/ros2_ws/install/local_setup.bash
    ```
* `source`して確認
  ```bash
  $ source ~/.bashrc
  $ ros2 pkg list | grep mypkg
  mypkg
  ```

---

### パブリッシャを持つノードの作成

* 機能: 数字をカウントしてトピック`/countup`を通じて送信
  * メッセージの型は16ビット符号つき整数
    * 参考にしたページ
        * [パブリッシャの書き方](https://index.ros.org/doc/ros2/Tutorials/Writing-A-Simple-Py-Publisher-And-Subscriber/#write-the-publisher-node)
        * [クラスなしで記述する方法](https://qiita.com/l1sum/items/b7393c34fb0127826f74)
* `~/ros2_ws/src/mypkg/mypkg`に`talker.py`を書く
  * コードの前半
    * オブジェクト: 今のところは様々な機能を持つ変数と考えて良い
  ```python
    1 import rclpy                     #ROS2のクライアントのためのライブラリ
    2 from rclpy.node import Node      #ノードを実装するためのNodeクラス（クラスは第10回で）
    3 from std_msgs.msg import Int16   #通信の型（16ビットの符号付き整数）
    4
    5 rclpy.init()
    6 node = Node("talker")            #ノード作成（nodeという「オブジェクト」を作成）
    7 pub = node.create_publisher(Int16, "countup", 10)   #パブリッシャのオブジェクト作成
    8 n = 0 #カウント用変数
    9
  ```
（下に続く）

>>>

* コードの後半
    ```python
     10 def cb():          #17行目で定期実行されるコールバック関数
     11     global n       #関数を抜けてもnがリセットされないようにしている
     12     msg = Int16()  #メッセージの「オブジェクト」
     13     msg.data = n   #msgオブジェクトの持つdataにnを結び付け
     14     pub.publish(msg)        #pubの持つpublishでメッセージ送信
     15     n += 1
     16
     17 node.create_timer(0.5, cb)  #タイマー設定
     18 rclpy.spin(node)            #実行（無限ループ）
  ```

---

### パッケージの設定

* パッケージに`talker.py`や依存するモジュールを登録
    * `package.xml`に利用するモジュールを登録
```
・・・
  <license>BSD-3-Clause</license>
  <exec_depend>rclpy</exec_depend>            <-追加
  <exec_depend>std_msgs</exec_depend>         <-追加
・・・
```
    * `setup.py`にスクリプト（正確にはエントリポイント）を登録
```python
・・・
        entry_points={
        'console_scripts': [
            'talker = mypkg.talker:main',
            #'listener = mypkg.listener:main', ←あとからコメントアウト
        ],
・・・
```


---

### ビルド

* 他に利用するパッケージを確認してインストール
    * `humble`はUbuntu 22.04用のROS2のバージョン
    * 20.04の場合は`foxy`
    ```bash
    $ cd ~/ros2_ws
    $ sudo rosdep install -i --from-path src --rosdistro humble -y 
    ```
* ビルド
    * `colcon build`して、その後`source`で設定を読み直し
    ```bash
    $ colcon build
    Starting >>> mypkg
    Finished <<< mypkg [2.55s]
    
    Summary: 1 package finished [3.06s]
    $ source ~/.bashrc
    ```

---

### 実行とメッセージの確認

* `ros2 run`で実行 
  ```
  $ ros2 run mypkg talker
  （なにも表示されない）
  ```
* 別の端末で`ros2`を使ってサブスクライブする
  ```
  $ ros2 topic echo /countup
  data: 13
  ---
  data: 14
  ---
  ・・・
  ```


---

### サブスクライバを持つノードの記述

* 機能: `/countup`からメッセージをもらって表示
  * `listener.py`という名前で
```python
  1 import rclpy
  2 from rclpy.node import Node
  3 from std_msgs.msg import Int16
  4
  5 def cb(msg):
  6     global node
  7     node.get_logger().info("Listen: %d" % msg.data)
  8
  9 rclpy.init()
 10 node = Node("listener")
 11 pub = node.create_subscription(Int16, "countup", cb, 10)
 12 rclpy.spin(node)
```

---

### <span style="text-transform:none">talker</span>と<span style="text-transform:none">listener</span>の実行

* `listener.py`について`setup.py`への記述
  * さきほどコメントに書いた部分の`#`を消す
* `colcon build`を済ませておく
```
端末1$ ros2 run mypkg talker
端末2$ ros2 run mypkg listener
[INFO] [Listener]: Listen: 142
[INFO] [Listener]: Listen: 143
・・・
```

---

## 5. まとめ

* ROS（ROS2）
  * 独立したプログラム（プロセス、ノード）を連携<br />　
* パッケージを作り、ふたつのノードを記述
  * 単にプログラムを走らせるだけでなく、様々な手続き
    * 最初は煩わしいが、人に使ってもらうモジュールを作りやすい。
