.. 翻訳者: Jun Okazaki

------------------
Gallery of Examples » Application from a .kv in a Template Directory
------------------

このドキュメンテーションはApplication from a .kv in a Template Directoryを日本語訳したものです。  
https://kivy.org/docs/examples/gen__application__app_with_kv_in_template1__py.html

.. image:: https://kivy.org/docs/_images/application__app_with_kv_in_template1__py1.png


この例は、.kvファイルのディレクトリを変更する方法を示しています。
「Hello from template1/test.ky」ボタンを参照する必要があります。

kivyはのAppのサブクラスTestAppをインスタンス化するように、変数kv_directoryが設定されています。
その後、Kivyはディレクトリ内のサブクラスの名前に一致する.kvファイルを暗黙的に検索し、template1/ test.kvファイルを見付けます。
ファイルにはroot widgetが含まれています。

------------------
application/app_with_kv_in_template1.pyファイル
------------------

.. code-block:: python
'''
Application from a .kv in a Template Directory
==============================================

This example shows how you can change the directory for the .kv file. You
should see "Hello from template1/test.ky" as a button.

As kivy instantiates the TestApp subclass of App, the variable kv_directory
is set. Kivy then implicitly searches for a .kv file matching the name
of the subclass in that directory, finding the file template1/test.kv. That
file contains the root widget.


'''

import kivy
kivy.require('1.0.7')

from kivy.app import App


class TestApp(App):
    kv_directory = 'template1'

if __name__ == '__main__':
    TestApp().run()


------------------
application/template1/test.kvファイル
------------------

.. code-block:: python
	#:kivy 1.0

	Button:
	    text: 'Hello from template1/test.kv'

