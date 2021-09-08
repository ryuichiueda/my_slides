# 設計製作論実習3（知能コース）

## イントロダクション

千葉工業大学 上田 隆一

<br />

<p style="font-size:50%">
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">
<img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a>
</p>

---

## 本コースでやること

* マニピュレーターCRANE-X7の制御
    * ムービー: https://lab.ueda.tech/?page=robotdesign3_2019_ros<br />　
    * 「今年はリモートでやるのにどうするの？」という話はあとで

---

## なにが学べるか

* マニピュレーターの制御理論・・・は二の次かも<br />　
* ソフトウェアの迅速な開発に主眼を置く
    * ROSやオープンソース、先輩のソースコードを使いこなして動くものをどんどん作っていく
    * 今年度は特に、実機がない状況、互いにリモートな状況でどのように開発を進めるか、がテーマ
        * 次ページ

---

## チームでのソフトウェア開発

* 企業ではハードが揃わないうちにソフトウェア開発が始まることがある
    * 最も分かりやすい例: 航空機
    * 入出力をしっかり定義して確認しながら開発を進める<br />　
* 複数人での開発をどうするか
    * コードを同時並行で記述する
    * しっかり予定や意図を（ストレスをかけることなく）伝える

---

## ツール

* LinuxあるいはWindowsのWSL（Windows Subsystem for Linux）環境
* ROS、Gazebo
* Slack、（Backlog？）
* GitHub

<iframe width="560" height="315" src="https://www.youtube.com/embed/ZMpj_mBggjw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

## 進め方

* 前半にちょっとした講義、後半作業
    * 作業時間をとるために前半の講義ビデオは事前に公開することも検討
* シミュレータを利用して開発
* 中間発表と期末発表で作成したソフトウェアを実機で動作させる
    * TAがリモートで
    * もしかしたら開発中もTAがリモートで動かしてくれるかも

---

## スケジュール（1/3）

* 第1回: ガイダンス
* 第2回: WSLとROSのセットアップ
* 第3回: チーム分け
    * （この頃ロボットシステム学でGitとGitHubをやります）

---

## スケジュール（2/3）

* 4〜8回: センサを使わないでロボットに仕事をさせる
    * 第4回: プレゼン
    * 第5回: 作業
    * 第6回: 作業
    * 第7回: 作業
    * 第8回: 中間発表

やりちらかしではなくてプロジェクトとして<br />まとめることを重視

---

## スケジュール（3/3）

* 9〜13回: センサを使ってロボットに仕事をさせる
    * 第9回: プレゼン
    * 第10回: 作業
    * 第11回: 作業
    * 第12回: 作業
    * 第13回: 最終発表

やりちらかしではなくてプロジェクトとして<br />まとめることを重視（重要なので2回言いました）

---

## 向いている人、向いていない人

* 向いている人
    * ROSの経験を積みたいと考えている人
    * マニピュレータをぐわんぐわん動かしたい人<br />　
* 向いていない人
    * コードを書くのが好きではない
        * チームで仕事をするので面白い仕事がまわってこないかも
    * オンライン上のコミュニケーションに興味がない人

