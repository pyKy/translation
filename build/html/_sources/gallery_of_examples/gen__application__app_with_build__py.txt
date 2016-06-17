.. 翻訳者: Jun Okazaki

------------------
事例のギャラリー >> build() と return を使用したアプリケーションの例
------------------

このドキュメンテーションはApplication example using build() + returnを日本語訳したものです。  
https://kivy.org/docs/examples/gen__application__app_with_build__py.html

.. image:: https://kivy.org/docs/_images/application__app_with_build__py1.png

self.rootを設定した場合、アプリケーションのウィジェットはbuild()にreturnを設定してビルドできます。


------------------
application/app_with_build.py ファイル
------------------

.. code-block:: python
	'''
	build() と return を使用したアプリケーションの例
	==========================================

	self.rootを設定した場合、アプリケーションのウィジェットはbuild()にreturnを設定してビルドできます。
	
	'''

	import kivy
	kivy.require('1.0.7')

	from kivy.app import App
	from kivy.uix.button import Button


	class TestApp(App):

	    def build(self):
	        # Button() をルートウィジェットに返却します
	        return Button(text='hello world')

	if __name__ == '__main__':
	    TestApp().run()


