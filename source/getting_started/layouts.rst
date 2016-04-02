.. 翻訳者: Kono Shinsaku

------------------
導入
------------------

このドキュメンテーションはKivyのGetting Started » Layoutsを日本語訳したものです。
https://kivy.org/docs/gettingstarted/layouts.html


.. Layouts are containers used to arrange widgets in a particular manner.
.. レイアウトは英語にする。
レイアウトは特定の記法でウィジェットを配置するために用いられるコンテナです。::

    アンカーレイアウト<https://kivy.org/docs/api-kivy.uix.anchorlayout.html#module-kivy.uix.anchorlayout>:::

        Widgetは、「top」、「bottom」、「left」、「right」または「センター」に固定することが可能です。

    ボックスレイアウト<https://kivy.org/docs/api-kivy.uix.boxlayout.html#module-kivy.uix.boxlayout>:::

        Widgetは横向きでも縦向きでも順番に配置されます。

    フロートレイアウト<https://kivy.org/docs/api-kivy.uix.floatlayout.html#module-kivy.uix.floatlayout>:::

        Widgetの数に制限はありません。

    レラティブレイアウト<https://kivy.org/docs/api-kivy.uix.relativelayout.html#module-kivy.uix.relativelayout>:::

        子に当たるウィジェットはレイアウトと比較して配置されます。

    グリッドレイアウト<https://kivy.org/docs/api-kivy.uix.gridlayout.html#module-kivy.uix.gridlayout>:::

        ウィジェットは、行と列のプロパティによって定義されるグリッドに配置されます。

    ページレイアウト<https://kivy.org/docs/api-kivy.uix.pagelayout.html#module-kivy.uix.pagelayout>:::

        単純な複数ページのレイアウトをつくるのに用いられます。現在のページから他ページへの即時移動を可能にします。

    スキャッターレイアウト<https://kivy.org/docs/api-kivy.uix.scatterlayout.html#module-kivy.uix.scatterlayout>:::

        ウィジェットは変換、回転・リサイズすることができます。"レラティブレイアウト"同様、子に当たるウィジェットはレイアウトと比較して配置されます。

    スタックレイアウト<https://kivy.org/docs/api-kivy.uix.stacklayout.html#module-kivy.uix.stacklayout>:::

        ウィジェットは、lr-tb(左上から右下へ)またはtb-lr(右下から左上へ)の順序で配置されます。

ウィジェットをレイアウトに加える時、レイアウトのタイプによって以下の特性が装置のサイズと位置を決定するのに用いられます:

    **サイズヒント** :親に当たるスペースでウィジェットのサイズをパーセンテージとして定義します。その値は、範囲0.0 から 1.0（0.01 = 1%、1.=100%）に制限されます。

    **ポジションヒント** :親と比較してウィジェットを置くのに用いられます。

**サイズヒント** と **ポジションヒント** は値が何らかに決まるときのみはウィジェットのサイズと位置を計算するのに用いられます。値を設定しない場合は、レイアウトはウィジェットの位置・大きさを設定しません。その場合、直接、画面を見て値（x、y、幅、高さ）を指定することができます。
