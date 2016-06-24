.. 翻訳者: Jun Okazaki

---------------------------------------
ウィジェットアニメーション
---------------------------------------

このドキュメンテーションはWidget animationを日本語訳したものです。  
https://kivy.org/docs/examples/gen__animation__animate__py.html

.. image:: https://kivy.org/docs/_images/animation__animate__py1.png

この例では、ボタンウィジェットを作成し、マルチパートのアニメーションを適用することを示しています。
クリックしたときにアニメーションで移動する「plop」と書かれたボタンが表示されるはずです。



animation/animate.py ファイル
---------------------------------------

.. code-block:: python
	'''
	ウィジェットアニメーション
	================

	この例では、ボタンウィジェットを作成し、マルチパートのアニメーションを適用することを示しています。
	クリックしたときにアニメーションで移動する「plop」と書かれたボタンが表示されるはずです。
	'''

	import kivy
	kivy.require('1.0.7')

	from kivy.animation import Animation
	from kivy.app import App
	from kivy.uix.button import Button


	class TestApp(App):

	    def animate(self, instance):
	        # animationオブジェクトを作成します。 このオブジェクトは保存され、
	        # 各呼び出しを再利用、別のウィジェットでも再利用できます。
	        # 「+=」 は逐次処理, また 「&=」 は並列処理です。
	        animation = Animation(pos=(100, 100), t='out_bounce')
	        animation += Animation(pos=(200, 100), t='out_bounce')
	        animation &= Animation(size=(500, 500))
	        animation += Animation(size=(100, 50))

	        # 「instance」引数で渡されたボタンにアニメーションを適用します。
	        # アニメーションが変更されていない、デフォルト「クリック」に注意してください。
	        #（ボタンの色を変更するマウスがダウンしている間）
	        animation.start(instance)

	    def build(self):
	        # on_pressハンドラとしてにanimate（）メソッドを指定するボタンを作成します。
	        button = Button(size_hint=(None, None), text='plop',
	                        on_press=self.animate)
	        return button

	if __name__ == '__main__':
	    TestApp().run()


