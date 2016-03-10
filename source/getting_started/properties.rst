.. 翻訳者:Jun Okazaki

==================================
入門
==================================
このドキュメンテーションはKivyのGetting Started » Propertiesを日本語訳したものです。  
https://kivy.org/docs/gettingstarted/properties.html


導入
================================

Kivyは、クラス内のプロパティを宣言するための新しい方法を紹介します。 前に：

.. code-block:: python

	class MyClass(object):
		def __init__(self):
			super(MyClass, self).__init__()
			self.numeric_var = 1

その後、Kivyプロパティを使用します：

.. code-block:: python

	class MyClass(EventDispatcher):
    	numeric_var = NumericProperty(1)

これらのプロパティはあなたを手助けするために`Observer パターン <https://ja.wikipedia.org/wiki/Observer_%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3>`を実装します。
* Kv language で定義されたウィジェットを簡単に操作します。
* ディスパッチ機能/コードのすべての変更を自動的に観察します。
* チェックして、値を検証します。
* メモリ管理を最適化します。


それらを使用するには、** クラスレベルで宣言する必要があります。
任意の他のメソッドではなく、直接クラスで宣言します。
プロパティは、自動的にインスタンス属性を作成するクラス属性です。

デフォルトでは、プロパティの状態/値が変化するさいに<プロパティ名>イベントを呼ぶことを各プロパティは提供します。

Kivyは、次のプロパティが用意されています。
 NumericProperty, StringProperty, ListProperty, ObjectProperty, BooleanProperty, BoundedNumericProperty, OptionProperty, ReferenceListProperty, AliasProperty, DictProperty,

詳細な説明については、`Properties. <https://kivy.org/docs/api-kivy.properties.html>`を見てみましょう。