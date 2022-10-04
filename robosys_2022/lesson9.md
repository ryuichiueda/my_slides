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
3. サービス
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

