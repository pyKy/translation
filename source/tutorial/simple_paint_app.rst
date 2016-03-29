--------------------------------------
シンプルなペイントアプリケーション
--------------------------------------

概要
--------

このドキュメンテーションはKivyのシンプルなペイントアプリのチュートリアルを日本語訳したものです。  
https://kivy.org/docs/tutorials/firstwidget.html

シンプルなペイントアプリケーション
--------------------------------------------

このチュートリアルでは、あなたが、最初にウィジェットを作成数る方法を教えています。
Kivyアプリケーションをプログラミングする際、あなたが特定の目的ためにカスタム要素で
新しいユーザーインターフェースを作成するための最も重要な知識を提供します。

基本的な考慮事項
----------------------------------

アプリケーションを作成する前に、自分自身で重要な3つの項目を確かめる必要があります。

* 私のアプリケーション・プロセスはどのようなデータですか？  
* どのように、私は視覚的にそのデータを表しますか？  
* どのようにユーザーがそのデータと対話しますか？  

たとえば、あなたが非常に単純な線画アプリケーションを記述したいのであれば、
ユーザーは指でスクリーンに描画することをたぶん望みます。 
それは、ユーザーがあなたのアプリケーションと対話する方法です。
その一方で、あなたのアプリケーションは後からユーザーの指があった位置を記憶します。、
そのため、あなたは後からそれらの位置の間に線を描くことができます。
つまり、指のあった点はあなたのデータであり、あなたがそれらの間に描く線は
あなたの視覚的な表現です。

Kivyでは、アプリケーションのユーザーインタフェースはウィジェットから構成されています。
あなたの画面に表示されるすべてが何らかの形でウィジェットによって描かれています。
しばしば、あなたは異なる文脈で書いたコードを再利用したいと思うでしょう。

.. A widget encapsulates data, defines the user’s interaction with that data and draws its visual representation. 
ウィジェットはデータをカプセル化し、そのデータとユーザの対話を定義します。そしてその視覚的な表現を描画します。
.. You can build anything from simple to complex user interfaces by nesting widgets. 
あなたは、入れ子のウィジェットによって、単純なインターフェースから複雑なインタフェースまで、何でも構築することができます。
.. There are many widgets built in, such as buttons, sliders and other common stuff. 
これには、ボタン、スライダーや他の一般的なもの、として構築された多くのウィジェットがあります。
.. In many cases, however, you need a custom widget that is beyond the scope of what is shipped with Kivy (e.g. a medical visualization widget).
しかし、多くの場合、Kivyに同胞されているウィジェットの範囲を超えたカスタムウィジェット(例えば、医療を視覚化するためのウィジェット)が必要となります。

.. So keep these three questions in mind when you design your widgets. 
あなたは、ウィジェットを設計するときに念頭においた３つの質問の意識を続けます。

.. Try to write them in a minimal and reusable manner 
最小限の再利用可能な方法でそれらを記述してみましょう。

(i.e. a widget does exactly what its supposed to do and nothing more. 
If you need more, write more widgets or compose other widgets of smaller widgets. 
We try to adhere to the Single Responsibility Principle).



.. code-block:: python

    from kivy.app import App
    from kivy.uix.widget import Widget
    
    class MyPaintWidget(Widget):
        pass
        
    class MyPaintApp(App):
        def build(self):
            return MyPaintWidget()
    
    if __name__ == '__main__':
        MyPaintApp().run()
    

