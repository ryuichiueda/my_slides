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
* もっといろんな人に使われる/使ってもらいたい場合は？
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
    * 「継続的インテグレーション」「継続的デリバリー」<br />「継続的デプロイ」<br />　
* <span style="color:red">「自動」でテストできることがキモ</span>
  * 自動化の仕組み: テストの終了ステータスが0なら<br />自動化のプログラムを起動

---

### <span style="text-transform:none">GitHub Actions</span>

* GitHubのCI/CDサービス
