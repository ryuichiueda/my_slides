# ロボットシステム学

## 第7回: GitHubでのテスト

千葉工業大学 上田 隆一

<br />

<p style="font-size:50%">
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">
<img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a>
</p>

---

## 今日やること

* GitHub Actions

---

### ソフトウェアのテスト

* 前回体験
  * 自分で使うソフトウェアならこれくらいで十分<br />　
* 仕事や人の使うソフトウェアの場合は？
  * 様々な環境に対応する必要性
    * Pythonのバージョン
    * Linuxだけじゃなく、他のOSは？
    * 自分の使っている環境（いろんなものが既にインストール済み）以外
  * 常に動作しているものが存在
    * 止めないでどうやって開発したものをマージするのか
  * ちゃんとテストしていると示したい

---

### テストの（も）できるウェブサービス

* そういうものが存在
  * [CircleCI](https://circleci.com/ja/)
  * [Travis CI](https://www.travis-ci.com/)
  * ・・・<br />　
* 基本的な動作
  1. GitHub等にpushされたコードをテスト
    * GitHub等のページに結果が表示されるように手配
  2. テストが通ったらコードをビルド
    * リリースしてユーザーが利用可能に
    * あるいは<span style="color:red">動いている本番のシステムに反映</span>


---

### 改めてテストの役割

* 抜け目のないテストにより
  * デバッグやリリースの際の手間の削減が可能
  * 「動いているシステムは怖くていじれない」からの脱却
    * 「継続的インテグレーション（CI）」「継続的デリバリー（CD）」<br />「継続的デプロイ」
    * CircleCIやTravis CIのサービス（<span style="color:red">CI/CDサービス</span>）のテスト機能: CIやCDを実現するためのもの<br />　
* <span style="color:red">「自動」でテストできることがキモ</span>
  * 自動化の仕組み: テストの終了ステータスが0<br />$\rightarrow$自動化のプログラムを起動

---

### <span style="text-transform:none">GitHub Actions</span>

* GitHubのCI/CDサービス
  * CircleCIやTravis CIより後発だが、GitHubにくっついていて使いやすいため、この講義で利用
  * テストをするときの使い方
    * リポジトリに`.github/workflows`というディレクトリを作成
    * その中に$\circ\circ$`.yml`というファイル（ワークフローファイル）を作成
      * とりあえず`test.yml`で
    * テストの手続きを記述（次ページ）
 ```bash
$ mkdir .github    #robosys2022のリポジトリのトップディレクトリで
$ cd .github/      #.githubディレクトリは隠れディレクトリになるので注意（lsで出てこない。ls -aで出る。）
$ mkdir workflows
$ cd workflows/
$ touch test.yml   #「./robosys2022/.github/workflows/test.yml」ができているとOK
 ```

---

### テストの手続きの記述

* 下の例のように記述
  * YAML（YAML Ain't a Markup Language.）形式
    * データをテキストファイルに記述したりやりとりしたりするときに使われる形式でROSでもよく用いられる。
    * インデントは半角空白2つが基本
      * 下のレベルのものが子のデータに。「`-`」は配列の要素。
    ```yaml
      name: test        #name: ワークフローの名前
      on: push          #on: いつこのワークフローを走らせるか
      jobs:             #走らせたい処理（ジョブ）のリスト
        test:           #testというジョブを作る
          runs-on: ubuntu-latest   #どの環境で動かすか
          steps:                   #手続きの記述
          - uses: actions/checkout@v3  #https://github.com/actions/checkoutのバージョン3を使用
          - name: All test             #このジョブの名前
            run: bash -xv ./test.bash  #テストのシェルスクリプトを走らせる
    ```
