# ロボットシステム学

## 第1回: イントロダクション

千葉工業大学 上田 隆一

<br />

<p style="font-size:50%">
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">
<img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a>
</p>

---

## 今日やること

* 講義の内容の理解（この動画）
* 環境の準備（別の資料か動画で）

---

## 本講義の目的

* 最終目標
    * ROSを使いこなす<br />　
* 途中の目標（こちらの方が大切かも）
    * ROSの「下」や「周辺」を理解して使いこなす
        * プログラミング
        * 通信
        * OSの仕組み
        * ライセンス
        * Git/GitHub
        * テスト

---

## 各回の内容（第1〜6回）

* 第1回: イントロダクションと環境の準備
* 第2回: Linux環境の使い方
* 第3回: PythonとLinux環境でのスクリプトの実行
* 第4回: GitとGitHub
* 第5回: 著作権とライセンス
* 第6回: ソフトウェアのテスト

---

## 各回の内容（第7〜13回）

* 第7回: ROSのノードと通信の基本
* 第8回: ROSの通信と型
* 第9回: Pythonのクラスとオブジェクト
* 第10回: ROSシステムのテスト
* 第11回: ROS2
* 第12回: 選択（動画を選んで視聴）
* 第13回: まとめ

---

## 評価

* 課題x2: 20点ずつ（ボーナス点あり）
* テスト: 60点

---

## 参考文献（<span style="text-transform:none">Python</span>）

* はじめての人向け
    * 森 巧尚: Python 1年生 体験してわかる！会話でまなべる！プログラミングのしくみ, 翔泳社, 2017. <br />　
* そうでない人向け
    * 各個人のレベルによりけりなので特に指定しませんが、文法の解説が中心のもの
    * 企業のエンジニアやビジネスマン向けのような応用中心のものは回避を

---

## 参考文献（<span style="text-transform:none">Linux</span>）

* 上田, 山田, 田代, 中村, 今泉, 上杉: 1日1問, 半年以内に習得 シェル・ワンライナー160本ノック, 技術評論社, 2021. 
    * 自著ですがおすすめ。
    * 少しずつLinux（のコマンドライン）が使えるようになっていく構成になっています。後半は難しいです。


---

## 環境の準備

* LinuxのCLI（コマンドラインインターフェース）環境を準備
    * GUI（グラフィックユーザインターフェース）はあってもよいですが、講義ではROSまで使いません。
        * 慣れるとCLIを使えたほうが楽。小型のロボットだとGUIが面倒。<br />　
* 次のいずれか
    * ネイティブのLinux環境（講義ではUbuntuを使用）
    * Windows Subsystem for Linux 2（これもUbuntu）
    * Ubuntuをインストールしたラズパイなど
    * 特殊なものは自己責任でご使用を
