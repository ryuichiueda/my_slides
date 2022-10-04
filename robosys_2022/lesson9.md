# ロボットシステム学

## 第9回: ROS2の通信と型

千葉工業大学 上田 隆一

<br />

<p style="font-size:50%">
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">
<img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a>
</p>

---

## 今日やること

1. launchファイル（前回の続き）
2. 独自のメッセージ型の作成
3. サービスの実装
4. パラメータ、actionlib

---

## 1. <span style="text-transform:none">launchファイル</span>

* launchファイル
  * 複数のノードを一度に立ち上げるもの
    * 他にもいろいろ可能だがとりあえず
  * 作っておくと、ノードごとに`ros2 run`しなくてよい

---

###  <span style="text-transform:none">launch</span>ファイルの作成（準備）

* パッケージのディレクトリに`launch`ディレクトリを作成
```
$ cd ~/ros2_ws/src/mypkg/
$ mkdir launch
```

* `setup.py`にローンチファイルの場所を記述
```
  2 import os                  #追加
  3 from glob import glob      #追加
（中略）
 11     data_files=[
（中略）                       #↓追加
 15        (os.path.join('share', package_name), glob('launch/*.launch.py'))
 16     ],
```
* `package.xml`に依存関係を記述
```
 12   <exec_depend>launch_ros</exec_depend>
```

---

###  <span style="text-transform:none">launch</span>ファイルの作成（記述）

* さきほど作った`launch`ディレクトリに設置
* 名前は`talk_listen.launch.py`としましょう
  * `setup.py`に`.launch.py`で終わると書いたので、それは守る
    ```python
      1 import launch
      2 import launch.actions
      3 import launch.substitutions
      4 import launch_ros.actions
      5
      6
      7 def generate_launch_description():
      8
      9     talker = launch_ros.actions.Node(
     10         package='mypkg',
     11         executable='talker',
     12         )
     13     listener = launch_ros.actions.Node(
     14         package='mypkg',
     15         executable='listener',
     16         output='screen'
     17         )
     18
     19     return launch.LaunchDescription([talker, listener])
    ```

---

### ローンチファイル（実行）

* `colcon build`して実行
```bash
$ ros2 launch mypkg talk_listen.launch.py
[INFO] [launch]: All log files can be found below /home/ubuntu/.ros/log/202...
[INFO] [launch]: Default logging verbosity is set to INFO
[INFO] [talker-1]: process started with pid [17158]
[INFO] [listener-2]: process started with pid [17159]
（かなり遅れてlistener.pyの出力がまとめて表示される場合あり）
（Ctrl+Cで終了）
```

---

## 2. 独自のメッセージ型の作成

* 前回の通信: ひとつ整数を送受信するだけ
* 意味のあるひとかたまりのデータをまとめて送るには？
  * 意味のあるひとかたまりのデータ: 構造体に相当するもの<br />　
* 方法
  * 既存の型を利用
    * `$ ros2 interface list`で一覧表示可能
  * 自分で型（と型のためのパッケージ）を作成
    * やってみましょう

---

### <span style="text-transform:none">msg</span>ファイルの作成

* 型を定義するパッケージを作成
  ```bash
  $ cd ~/ros2_ws/src
  $ ros2 pkg create --build-type ament_cmake person_msg
  $ cd person_msg
  ### package.xmlを前回のように編集 ###
  $ mkdir msg
  $ cd msg
  ```
* `msg`下に、次のような`Person.msg`というファイルを設置
    ```
    string name
    uint8 age
    ```
   * このファイルから、各言語で使える構造体のようなものが生成される

---

### ビルドの準備

* パッケージのトップディレクトリにある`CMakeList.txt`を編集
  * `find_package(ament_cmake REQUIRED)`の下あたりに、次の4行を記述
    ```cmake
    find_package(rosidl_default_generators REQUIRED)
    rosidl_generate_interfaces(${PROJECT_NAME}
      "msg/Person.msg"
    )
    ```
* `package.xml`も編集
  * `buildtool_depend`の下あたりに、次の3行を追加
    ```xml
    <build_depend>rosidl_default_generators</build_depend>
    <exec_depend>rosidl_default_runtime</exec_depend>
    <member_of_group>rosidl_interface_packages</member_of_group>
    ```

---

### ビルド

* ビルドして環境に反映
  ```bash
  $ cd ~/ros2_ws
  $ colcon build
  （略）
  ---
  Finished <<< mypkg [0.57s]
  Finished <<< person_msg [2.32s]                     
  
  Summary: 2 packages finished [2.51s]
    1 package had stderr output: mypkg
  ```
* `source`して、型が利用できるようになっているか確認
  ```bash
  $ source ~/.bashrc
  $ ros2 interface show person_msg/msg/Person
  string name
  uint8 age
  ```

---

### <span style="text-transform:none">Person型の利用（publish）</span>

* `talker.py`からPerson型のメッセージを送信
    ```python
     1 import rclpy
     2 from rclpy.node import Node
     3 from person_msgs.msg import Person #使う型を変更
     4 
     5 rclpy.init()
     6 node = Node("talker")
     7 pub = node.create_publisher(Person, "person", 10) #変更
     8 n = 0
     9 def cb():
    10     global n
    11     msg = Person()         #受信するデータの型を変更
    12     msg.name = "上田隆一"  #msgファイルに書いた「name」
    13     msg.age = n            #msgファイルに書いた「age」
    14     pub.publish(msg)
    15     n += 1
    16 
    17 node.create_timer(0.5, cb)
    18 rclpy.spin(node)
    ```
   * ビルドして実行し、`ros2 topic echo`で確認のこと
  

---

### <span style="text-transform:none">Person型の利用（subscribe）</span>

* `listener.py`でPerson型のメッセージを受信、表示
  ```python
   1 import rclpy
   2 from rclpy.node import Node
   3 from person_msgs.msg import Person
   4
   5 def cb(msg):
   6     global node
   7     node.get_logger().info("Listen: %s" % msg) #変更
   8
   9 rclpy.init()
  10 node = Node("listener")
  11 pub = node.create_subscription(Person, "person", cb, 10) #変更
  12
  13 rclpy.spin(node)
  ```
  * `ros2 launch`で動作確認を

---

## 3. サービスの実装

* トピックは基本的にいつpublishしてもよいし、<br />いつsubscribeしてもよい
  * ノード同士が干渉することがない<br />　
* ノードとノードが直接データや仕事の依頼をやりとりしたいときは？
  * サービスの利用<br />　
* サービス
  * あるノードが別のノードに仕事を依頼する仕組み<br />　
* サービスを実装してみましょう

---

### 実装するサービス

* 人の名前を送ったら、その人の年齢を返すサービス
  * `listener`が依頼する側
  * `talker`が依頼する側

---

### <span style="text-transform:none">srvファイルの作成</span>

* 準備: `person_msgs`に`srv`というディレクトリを作成
  * `msg`と同じところに
  * 中に、次のような`Query.srv`を置く
    ```
    string name
    ---
    uint8 age
    ```
    * `---`の上: サービスを呼び出すときに呼び出し側が渡すデータ
    * `---`の下: サービスの実行後に呼び出し側が受け取るデータ


---

### ビルド

* `CMakeList.txt`に`Query.srv`を追加
  ```cmake
   15 rosidl_generate_interfaces(${PROJECT_NAME}
   16   "msg/Person.msg"
   17   "srv/Query.srv"   # 追加
   18 )
   ```
* ビルド、`source`、確認
  ```bash
  $ cd ~/ros2_ws/
  $ colcon build
  （略）
  $ source ~/.bashrc 
  $ ros2 interface show person_msgs/srv/Query
  string name
  ---
  uint8 age
   ```

---

### サービスのためのコールバック関数の実装

* `talker`側に、サービスが呼び出されたときの処理を書く
  * `talker.py`
    ```python
      1 import rclpy
      2 from rclpy.node import Node
      3 from person_msgs.srv import Query #使う型を変更
      4 
      5 def cb(request, response):
      6     if request.name == "上田隆一":
      7         response.age = 44
      8     else:
      9         response.age = 255
     10 
     11     return response
     12 
     13 rclpy.init()
     14 node = Node("talker")
     15 srv = node.create_service(Query, "query", cb) #サービスの作成
     16 rclpy.spin(node)
    ```

---

### 動作確認

* `ros2 service`を使用
* `talker`を立ち上げてからサービスの存在を確認し、<br />呼び出してみる
  ```bash
  ### 確認 ###
  $ ros2 service list
  /query                  <- 表示されるか確認
（以下略）
  ### 実行 ###
  $ ros2 service call /query person_msgs/srv/Query "name: 上田隆一"
  requester: making request: person_msgs.srv.Query_Request(name='上田隆一')
  
  response:
  person_msgs.srv.Query_Response(age=44)
  
  $ ros2 service call /query person_msgs/srv/Query "name: しらない 人"
  （略）
  response:
  person_msgs.srv.Query_Response(age=255)
  ```

---

### ノードからのサービス呼び出し




