.. 翻訳者: Jun Okazaki

------------------------------------
Widowsでのパッケージの方法
------------------------------------

このドキュメンテーションはKivyのProgramming Guide » Create a package for Windowsを日本語訳したものです。
https://kivy.org/docs/guide/packaging-windows.html#create-a-package-for-windows

この文章はkivy 1.9.1以降のバージョンが対象です。


Windowsプラットフォーム用のアプリケーションのパッケージは、Windows OS内でできます。 
次のプロセスはKivy wheelsのインストールをwindowsにインストールし、インストールの終了までテストされています。 

パッケージは実行したPythonのバージョンに応じて32bit,64bitのどちらかになります。


要件
------------------------------------
	・Kivyの最新バージョン（Windowsへのインストールの記載の通りインストール ）。
	・pyInstallerは3.1以上（ pip install --upgrade pyinstaller ）。


pyInstallerのデフォルトのフック

このセクションでは、kivyフックを含むためpyInstallerの（> = 3.1）に適用されます。
デフォルトのフックを上書きするには次の例のように少し修正する必要があります。 
「デフォルトフックを上書き」を参照してください。 。


シンプルなアプリをパッケージ化
------------------------------------
この例では、touchtracerサンプルプロジェクトをパッケージ化し、カスタムアイコンを埋め込みます。
wheelを使用してインストールしている場合、kivyのサンプルコードの場所はpython\\share\\kivy-examplesに
githubのソースコードをインストールして使用するときはkivy\\examplesにあります 。
サンプルコードに至る絶対パスexamples-pathパスを参照します。
touchtracerの例があるexamples-path\\demo\\touchtracerとメインファイルはmain.pyです。

以後翻訳中



