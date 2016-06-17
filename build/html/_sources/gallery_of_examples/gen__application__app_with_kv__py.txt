.. 翻訳者: Jun Okazaki

------------------
事例のギャラリー >>  アプリケーションを.kv fileからビルドする
------------------

このドキュメンテーションはApplication built from a .kv fileを日本語訳したものです。  
https://kivy.org/docs/examples/gen__application__app_with_kv__py.html

.. image:: https://kivy.org/docs/_images/application__app_with_kv__py1.png


アプリケーションで暗黙的に.kvファイルを使用して表示しています。「Hello from test.kv」と表示されたフルスクリーンボタンが表示されるはずです。
Kivyは、アプリのサブクラスのインスタンスを作成した後、暗黙的に.kvファイルを検索します。
アプリのサブクラスの名前がTestAppである場合kivyが「test.kv」をロードすることを意味するため、ファイルtest.kvが選択されます。ファイルは、ルートウィジェットが含まれています。

------------------
application/app_with_kv.pyファイル
------------------

.. code-block:: python
'''
Application built from a  .kv file
==================================

アプリケーションで暗黙的に.kvファイルを使用して表示しています。「Hello from test.kv」と表示されたフルスクリーンボタンが表示されるはずです。

Kivyは、アプリのサブクラスのインスタンスを作成した後、暗黙的に.kvファイルを検索します。
アプリのサブクラスの名前がTestAppである場合kivyが「test.kv」をロードすることを意味するため、ファイルtest.kvが選択されます。ファイルは、ルートウィジェットが含まれています。
'''

import kivy
kivy.require('1.0.7')

from kivy.app import App


class TestApp(App):
    pass

if __name__ == '__main__':
    TestApp().run()


------------------
application/test.kvファイル
------------------

.. code-block:: python
	#:kivy 1.0

	Button:
	    text: 'Hello from test.kv'