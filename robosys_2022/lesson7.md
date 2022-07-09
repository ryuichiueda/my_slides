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
  * 常に動作しているものが存在
    * 止めないでどうやって開発したものをマージするのか
  * ちゃんとテストしていると示したい

---

### CI/CDサービス

* CI: continuous integration、継続的インテグレーション
  * 常に
* CD: continuous delivery、継続的デリバリー


---

### <span style="text-transform:none">GitHub Actions</span>

* GitHubのCI/CDサービス
