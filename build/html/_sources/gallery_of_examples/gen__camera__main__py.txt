.. 翻訳者: Jun Okazaki

------------------------------------
Gallery of Examples » Camera Example
------------------------------------

このドキュメンテーションはGallery of Examples » Camera Exampleを日本語訳したものです。  
https://kivy.org/docs/examples/gen__camera__main__py.html

.. image:: https://kivy.org/docs/_images/camera__main__py1.png

この例では、カメラの簡単な使用方法を示しています。
「Play」ボタンでウィンドウでのカメラ表示オンとオフに切り替えます。
カメラを見つけていない、またはgstreamerのがインストールされていない場合は、KV言語処理中に例外をスローすることに注意してください。


camera/main.pyファイル
-----------------------------

.. code-block:: python
'''
Camera Example
==============

This example demonstrates a simple use of the camera. It shows a window with
a buttoned labelled 'play' to turn the camera on and off. Note that
not finding a camera, perhaps because gstreamer is not installed, will
throw an exception during the kv language processing.

'''

# Uncomment these lines to see all the messages
#from kivy.logger import Logger
#import logging
#Logger.setLevel(logging.TRACE)

from kivy.app import App
from kivy.lang import Builder

kv = '''
BoxLayout:
    orientation: 'vertical'
    Camera:
        id: camera
        resolution: (640, 480)
        play: False
    ToggleButton:
        text: 'Play'
        on_press: camera.play = not camera.play
        size_hint_y: None
        height: '48dp'
'''


class TestCamera(App):
    def build(self):
        return Builder.load_string(kv)

TestCamera().run()
