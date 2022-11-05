# ロボットシステム学

## 第10回: ROSシステムのテスト

千葉工業大学 上田 隆一

<br />

<p style="font-size:50%">
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">
<img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a>
</p>

---

## 今日やること

1. 第8回の内容のコードをGitHubから取り出す
  * 第9回でないので注意
2. 第8回のlistener、talkerの動作を確認するテストを書く
  * シェルスクリプトで（非標準かもしれない）
3. GitHub Actionsでテストを動かす
4. まとめ
<br /> 　
<div style="font-size:90%">
注意: 第1回の予告での第10回と第11回を入れ替えました<br />
（テストを書いてからのほうがコードを変更しやすいので）
</div>

---

## 1. 第8回のコードの取り出し

* ちゃんとGitHubでブランチを分けてましたか？ or コミットしてましたか？
    * ブランチを分けていない場合（過去のコミットから取り出し）
        ```bash
        $ git log
        （中略）
          commit 3291472c08ad9407217b8ebf54b971ea4eefe7d6
          Author: Ryuichi Ueda <ryuichiueda@gmail.com>
          Date:   Tue Oct 4 11:18:56 2022 +0900
          
              Change for using Person
          
          commit a906a47eda6962306edc91f3e72175940d2a7505    <- これっぽい
          Author: Ryuichi Ueda <ryuichiueda@gmail.com>
          Date:   Tue Oct 4 11:01:22 2022 +0900
          
              Update
        （中略）
        $ git checkout a906a47                 <- コミットをチェックアウト
        $ git switch -c lesson10               <- 新たにブランチを作成
        Switched to a new branch 'lesson10'    <- このあと動作確認しましょう
        ```

---

## 2. テストの記述

* ノードを立ち上げ、`listener`の出力を検査
    * たぶん非標準なのですがシェルスクリプトで
    * `test`ディレクトリにシェルスクリプトを設置
         ```bash
         $ cd ~/ros2_ws/src/mypkg   # パッケージのトップディレクトリへ
         $ cd test                  # testディレクトリに移動
         $ vi test.bash             # 記述（次ページ）
         ```
    * `test`ディレクトリには他のテストがすでに存在しているので、あとで見ておきましょう

---

### <span style="text-transform:none">test.bash</span>の内容

* 10秒間ノードを実行して、`listener`が出力するべき行を探すという簡単なものを書く
    * 出力されているべき行: `[listener-2] ...: Listen: 10`という行
    * 書いたら動作確認を
        * `grep`の行を変えて終了ステータスを観察するなど
        ```bash
         1 #!/bin/bash
         2
         3 dir=~
         4 [ "$1" != "" ] && dir="$1"   #引数があったら、そちらをホームに変える。
         5
         6 cd $dir/ros2_ws
         7 colcon build
         8 source $dir/.bashrc
         9 timeout 10 ros2 launch mypkg talk_listen.launch.py > /tmp/mypkg.log
        10
        11 cat /tmp/mypkg.log |
        12 grep 'Listen: 10'
        ```

---

## 3. <span style="text-transform:none">GitHub</span>でテストを動かす

* ROSのようなミドルウェア上で動くソフトのテスト
    * 環境を整備しないとソフトが動かないので面倒<br />　
* CIサイトでの方法
    * その1: 毎回インストールする
    * その2: インストール済みのものをもってくる
        * 今回はこっち
        * 「コンテナ」を利用

---

### コンテナ

* 今回は「ソフトウェア一式を箱に閉じ込めたもの」という理解で十分
    * 詳しくは2021年度までの講義資料で
    * https://youtu.be/Utvf4YmMJpk <br />　
* テストに利用するコンテナ 
    * https://hub.docker.com/repository/docker/ryuichiueda/ubuntu22.04-ros2
        * 講師がUbuntu 22.04 LTSにROS2をセットアップしたもの
        * ワークスペース: コンテナ内の`/root/ros2_ws/`に存在
    * これをGitHub Actionsでダウンロードして使用

---

### ワークフローファイルの準備

* `test.yml`という名前で
    * どこに置いていいかわからない場合は第7回を復習
    * ディレクトリ構成がややこしく、うまくいかないと何時間もバグがとれないのですぐ質問を
        ```bash
         1 name: test
         2 on: push
         3 jobs:
         4   test:
         5     runs-on: ubuntu-22.04
         6     container: ryuichiueda/ubuntu22.04-ros2:latest #前ページのコンテナを使うという宣言
         7     steps:
         8       - uses: actions/checkout@v2    #コンテナのカレントディレクトリにリポジトリを配置
         9       - name: build and test
        10         run: |
        11           rsync -av ./ /root/ros2_ws/src/mypkg/    # リポジトリの下をros2_ws下にコピー
        12           cd /root/ros2_ws
        13           rosdep update                                            #14行目のために必要
        14           rosdep install -i --from-path src --rosdistro humble -y  #不要だけど念のため
        15           bash -xv ./src/mypkg/test/test.bash /root
        ```
        * 実行例: https://github.com/ryuichiueda/mypkg/actions/runs/3398928719

---

## 4. まとめ

* ROS2のパッケージをテストした
    * 今回はいろいろ応用が効くようにシェルスクリプトで
        * 補足: もっと便利なやりかたがあるはずなので、興味があれば調査を<br />　
* ROS2のパッケージをGitHub Actionsでテストした
    * 環境を整えるワークフローを書くのが結構面倒だが、1度ワークフローを完成されられればあとは簡単
    * GUIつきのROSパッケージでもテスト可能
        * ROS1の例: [ryuichiueda/emcl2](https://github.com/ryuichiueda/emcl2)
