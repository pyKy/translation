.. 翻訳者: Jun Okazaki

------------------
Kivyの基礎
------------------

このドキュメンテーションはKivyのProgramming Guide » Kivy Basicsを日本語訳したものです。
https://kivy.org/docs/gettingstarted/layouts.html


-----------------------
Kivy環境のインストール
-----------------------

Kivyは、pygame、gstreameの、PIL、Cairo、その他多くのPythonライブラリに依存します。これらは、すべてが必要ではありませんが、取り組んでいるプラットフォームにインストールするには、面倒です。 WindowsやMacOS Xで解凍して使用することができるポータブルなパッケージを提供します。

* Windowsでのインストール
* OS Xでのインストール
* Linuxでのインストール

すべてのライブラリを自分でインストールする場合は、少なくともPythonとpygameのを持っていることを確認してください。典型的なpipでのインストールは次のようになります。

.. code-block:: python

	pip install cython
	pip install hg+http://bitbucket.org/pygame/pygame
	pip install kivy

開発バージョンは、gitからインストールできます。

.. code-block:: python

	git clone https://github.com/kivy/kivy
	make

-----------------------
アプリケーションを作る
-----------------------

kivyアプリケーションを作成するのは簡単です。

* Appクラスをサブクラス化
* Widgetのインスタンス（自分のWidgetツリーのルート）を返すように、そのbuild（）メソッドを実装します
* クラスをインスタンス化し、run（）メソッドを呼び出します。

最小限のアプリケーションの例は次のとおりです

.. code-block:: python

	import kivy
	kivy.require('1.0.6') # replace with your current kivy version !

	from kivy.app import App
	from kivy.uix.label import Label


	class MyApp(App):

	    def build(self):
	        return Label(text='Hello world')


	if __name__ == '__main__':
	    MyApp().run()


例をmain.pyとしてテキストファイルに保存し、実行できます。

-----------------------
Kivyアプリのライフサイクル
-----------------------

最初に、Kivyアプリのライフサイクルについて詳しくなってみましょう。

.. image::https://kivy.org/docs/_images/Kivy_App_Life_Cycle.png

お分かりのように、すべての意図や目的のために、Appのエントリポイントは、run()メソッドで、私たちの場合には「MyApp().run()”」です。三行目で改めてこれを宣言することで開始できるようになります：

.. code-block:: python

	from kivy.app import App

あなたのAppのベースクラスはAppクラスを継承することが必須です。それはkivy_installation_dir/ kivy/ app.pyに存在します。

ノート
KivyAppクラスが何をするのかをより深く掘り下げたい場合は、そのファイルを開きます。コードを開いて、ファイルを読むことをお勧めします。 Kivyは、Pythonベースで、ドキュメントのためにSphinを使用していますので、各クラスのドキュメントは、実際のファイルになっています。

2行目付近


.. code-block:: python

	from kivy.uix.label import Label

ここで注意すべき重要な点は、パッケージ/クラスがレイアウトされている方法です。 UNIXモジュールは、レイアウトやウィジェットなどのユーザーインターフェイス要素を保持している部分です。

5行目に移動して、

.. code-block:: python

	class MyApp(App):

KivyAppの基本クラスを定義しているところです。アプリでMyAppの名前を変更する必要がありますする。

さらに7行目に、

.. code-block:: python

	def build(self):

KivyAppのLife Cycleを図で強調されているように、これは初期化しRoot Widgetを返す関数です。


8行目は何をすべきかです：

.. code-block:: python

	return Label(text='Hello world')


ここでは、テキストが「Hello World」となるLabelを初期化し、そのインスタンスを返します。このラベルは、このAppのRoot Widgetになります。

ノート

Pythonはにインデントを使用しコードブロックを表しています。したがって、9行目のクラスと関数定義の終わりに、上記のコード内容であることに注意してください。

11行目と12行目でアプリの実行を行います。

.. code-block:: python

	if __name__ == '__main__':
    MyApp().run()


MyAppクラスは初期化され、そのrun（）メソッドが呼び出されます。Kivyアプリケーションを初期化し、起動します。


-----------------------
アプリケーションの実行
-----------------------
アプリケーションを実行するには、使用しているオペレーティングシステムの指示に従ってください。

Linux

Linux上でKivyアプリケーションを実行するための指示に従ってください。

.. code-block:: python

	$ python main.py

Linux

Linux上でKivyアプリケーションを実行するための指示に従ってください。

.. code-block:: python

	$ python main.py

Windows

Windows上でKivyアプリケーションを実行するための指示に従ってください。

.. code-block:: python

	$ python main.py
	# or
	C:\appdir>kivy.bat main.py


Mac OS X

Mac OS X上でKivyアプリケーションを実行するための指示に従ってください。

.. code-block:: python

	$ kivy main.py


Android

アプリケーションをAndroid上で実行できるようにするには、いくつかの補完的なファイルを必要とします。
参考のためにAndroid用のパッケージの作成を参照してください。


一つのラベル（テキスト「こんにちは世界で）でウィンドウ全体を占めているウィンドウが開きます。
これで設定は完了です。

.. image:: https://kivy.org/docs/_images/quickstart.png


-----------------------
アプリケーションのカスタマイズ
-----------------------

ユーザー名/パスワードを入力するシンプルなページを加えて、アプリケーションを少し拡張しましょう。

.. code-block:: python

	from kivy.app import App
	from kivy.uix.gridlayout import GridLayout
	from kivy.uix.label import Label
	from kivy.uix.textinput import TextInput


	class LoginScreen(GridLayout):

	    def __init__(self, **kwargs):
	        super(LoginScreen, self).__init__(**kwargs)
	        self.cols = 2
	        self.add_widget(Label(text='User Name'))
	        self.username = TextInput(multiline=False)
	        self.add_widget(self.username)
	        self.add_widget(Label(text='password'))
	        self.password = TextInput(password=True, multiline=False)
	        self.add_widget(self.password)


	class MyApp(App):

	    def build(self):
	        return LoginScreen()


	if __name__ == '__main__':
	    MyApp().run()


2行目でGridlayoutをインポートします。

.. code-block:: python

	from kivy.uix.gridlayout import GridLayout

このクラスは9行目で定義された Root Widget（LoginScreen）のベースとして使用されます。

.. code-block:: python

	class LoginScreen(GridLayout):


12行目のLoginScreenクラスで、メソッド__init __（）はwidgetを追加するには、それらのビヘイビアを定義するようにオーバーロードします。

.. code-block:: python

	def __init__(self, **kwargs):
		super(LoginScreen, self).__init__(**kwargs)


一つ忘れてはいけないのは、オーバーロードされた元のクラスの機能を実装するためには、superを呼び出すことです。
また、時々はsuperの内部的な呼び出しの中に** kwargsからは省略しない良い方法があることに注意してほしいです。


15行めに移動して、枠の上から、

.. code-block:: python

	self.cols = 2
	self.add_widget(Label(text='User Name'))
	self.username = TextInput(multiline=False)
	self.add_widget(self.username)
	self.add_widget(Label(text='password'))
	self.password = TextInput(password=True, multiline=False)
	self.add_widget(self.password)


GridLayoutの2つの列にラベルと、ユーザー名とパスワードのTextInput を追加し、子として管理します。

上記のコードを実行すると、以下のようなウィンドウが得られます。

.. image:: https://kivy.org/docs/_images/guide_customize_step1.png

ウィンドウリサイズを試してみると、画面上のwidgetは何もしなくても、ウィンドウのサイズに応じてサイズ調整していることがわかります。widgetはデフォルトでサイズ変更を使用しているためです。


上記のコードは、ユーザからの入力を処理しないと、バリデーションや何らかの動作を行いません。
今後のセクションではこれらとwidgetのサイズや位置をより深く掘り下げていきます。

-----------------------
特異なプラットフォーム
----------------------

ターミナルアプリケーションを開くとkivy環境変数を設定します。

Windowsでは、kivy.batをダブルクリックすると、端末は既に設定されている必要なすべての変数で開かれます。

nix*システムでは、kivyがグローバルにインストールされていない場合はお好みのターミナルを開き、

.. code-block:: python

	export python=$PYTHONPATH:/path/to/kivy_installation
