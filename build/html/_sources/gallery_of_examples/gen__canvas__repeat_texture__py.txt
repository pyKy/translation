.. 翻訳者: Jun Okazaki

------------------------------------------------------
Repeat Texture on Resize
------------------------------------------------------

このドキュメンテーションはGallery of Examples » Repeat Texture on Resizeを日本語訳したものです。  
https://kivy.org/docs/examples/gen__canvas__repeat_texture__py.html

.. image:: https://kivy.org/docs/_images/canvas__repeat_texture__py1.png


この例では、ウィンドウに文字 'K'（mtexture1.png）を64回繰り返します。
現在サイズを示すラベルに沿って、8行8列のｐ白色のKの文字が表示されます。
ウィンドウのサイズを変更するしても、表示は8×8のままです。
この例では、色付きの背景を持つラベルが含まれています。

注：mtexture1.pngは白色で'K'で背景が透明なので目視が難しいです。


canvas/repeat_texture.py
-------------------------

.. code-block:: python
'''
Repeat Texture on Resize
========================

This examples repeats the letter 'K' (mtexture1.png) 64 times in a window.
You should see 8 rows and 8 columns of white K letters, along a label
showing the current size. As you resize the window, it stays an 8x8.
This example includes a label with a colored background.

Note the image mtexture1.png is a white 'K' on a transparent background, which
makes it hard to see.
'''

from kivy.app import App
from kivy.uix.image import Image
from kivy.uix.label import Label
from kivy.properties import ObjectProperty, ListProperty
from kivy.lang import Builder

kv = '''
<LabelOnBackground>:
    canvas.before:
        Color:
            rgb: self.background
        Rectangle:
            pos: self.pos
            size: self.size

FloatLayout:
    canvas.before:
        Color:
            rgb: 1, 1, 1
        Rectangle:
            pos: self.pos
            size: self.size
            texture: app.texture

    LabelOnBackground:
        text: '{} (try to resize the window)'.format(root.size)
        color: (0.4, 1, 1, 1)
        background: (.3, .3, .3)
        pos_hint: {'center_x': .5, 'center_y': .5 }
        size_hint: None, None
        height: 30
        width: 250

'''


class LabelOnBackground(Label):
    background = ListProperty((0.2, 0.2, 0.2))


class RepeatTexture(App):

    texture = ObjectProperty()

    def build(self):
        self.texture = Image(source='mtexture1.png').texture
        self.texture.wrap = 'repeat'
        self.texture.uvsize = (8, 8)
        return Builder.load_string(kv)

RepeatTexture().run()
    

Image canvas/mtexture1.png　画像ファイル
----------------------------------------

.. image:: https://kivy.org/docs/_images/mtexture1.png


