.. 翻訳者: Jun Okazaki

------------------
Non-widget stuff
------------------

このドキュメンテーションはKivyのGetting Started » Non-widget stuffを日本語訳したものです。
https://kivy.org/docs/gettingstarted/layouts.html

.. csv-table::
   :widths: 10, 10

   "Animation は、目標時間内に目標の値にWidgetのプロパティ（サイズ/ 位置/センター等）を変更するのに使用されます。様々なtransitionが設けられています。Widgetをアニメーション化し、非常に滑らかなUIビヘイビアを構築するために使用することができます。", .. image:: https://kivy.org/docs/_images/gs-animation.gif
   "Atlas はテクスチャマップを管理するためのクラスです（例；1枚の画像に複数のテクスチャをパッキングする）。これはロードされたイメージの数を減らすため、アプリケーションの起動を高速化することができます。", .. image:: https://kivy.org/docs/_images/gs-atlas.png
   "Clockは設定された時間間隔でジョブをスケジュールするために便利な方法を提供します。kivyイベントループをブロックするsleep（）より好ましいです。これらの間隔の前または後に、OpenGLの描画命令に対して設定できます。またClockは、次のフレームの前に一度だけ呼ばれる組み合わさったトリガーイベントを作成する方法を提供します。",  schedule_once()  schedule_interval()  unschedule()  create_rigger()"
   "URLRequestはイベントループをブロックせず、コールバックの結果と進捗状況を、管理していない非同期リクエストのために有用です。", 
