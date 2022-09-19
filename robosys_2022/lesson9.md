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
2. トピックの型
3. サービス
4. パラメータ、actionlib

---

## 1. <span style="text-transform:none">launchファイル</span>

* launchファイル
  * 複数のノードを一度に立ち上げるもの
    * 他にもいろいろ可能だがとりあえず
  * 作っておくと、ノードごとに`ros2 run`しなくてよい

---

###  launchファイルの作成

* パッケージのディレクトリに`launch`ディレクトリを作成

```bash
$ cd ~/ros2_ws/src/mypkg/
$ mkdir launch
$ cd launch/
```

