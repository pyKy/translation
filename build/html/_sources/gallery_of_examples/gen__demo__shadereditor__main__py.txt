.. 翻訳者: Jun Okazaki

------------------------------------
Live Shader Editor
------------------------------------

このドキュメンテーションはGallery of Examples » Live Shader Editorを日本語訳したものです。  
https://kivy.org/docs/examples/gen__demo__shadereditor__main__py.html

.. image:: https://kivy.org/docs/_images/demo__shadereditor__main__py1.png

頂点とフラグメントを編集するライブエディタを提供します。
左側に編集可能なペインが2つ、右側に大きなkivyのロゴを持つウィンドウが表示されます。
上部のペインには、バーテックスシェーダで、ボトムはフラグメントシェーダです。
shadereditor.kvファイルは、インタフェースについて説明します。

いずれかのシェーダへの各キーストロークで、宣言が追加され、シェーダがコンパイルされます。
エラーがない場合は、画面を更新します。
それ以外の場合、エラーは端末内のログメッセージとして表示します。

demo/shadereditor/main.py ファイル
------------------------------------

.. code-block:: python
'''
Live Shader Editor
==================

This provides a live editor for vertex and fragment editors.
You should see a window with two editable panes on the left
and a large kivy logo on the right.The top pane is the
Vertex shader and the bottom is the Fragment shader. The file shadereditor.kv
describes the interface.

On each keystroke to either shader, declarations are added and the shaders
are compiled. If there are no errors, the screen is updated. Otherwise,
the error is visible as logging message in your terminal.
'''


import sys
import kivy
kivy.require('1.0.6')

from kivy.app import App
from kivy.uix.floatlayout import FloatLayout
from kivy.core.window import Window
from kivy.factory import Factory
from kivy.graphics import RenderContext
from kivy.properties import StringProperty, ObjectProperty
from kivy.clock import Clock
from kivy.compat import PY2

fs_header = '''
#ifdef GL_ES
    precision highp float;
#endif

/* Outputs from the vertex shader */
varying vec4 frag_color;
varying vec2 tex_coord0;

/* uniform texture samplers */
uniform sampler2D texture0;

/* custom one */
uniform vec2 resolution;
uniform float time;
'''

vs_header = '''
#ifdef GL_ES
    precision highp float;
#endif

/* Outputs to the fragment shader */
varying vec4 frag_color;
varying vec2 tex_coord0;

/* vertex attributes */
attribute vec2     vPosition;
attribute vec2     vTexCoords0;

/* uniform variables */
uniform mat4       modelview_mat;
uniform mat4       projection_mat;
uniform vec4       color;
'''


class ShaderViewer(FloatLayout):
    fs = StringProperty(None)
    vs = StringProperty(None)

    def __init__(self, **kwargs):
        self.canvas = RenderContext()
        super(ShaderViewer, self).__init__(**kwargs)
        Clock.schedule_interval(self.update_shader, 0)

    def update_shader(self, *args):
        s = self.canvas
        s['projection_mat'] = Window.render_context['projection_mat']
        s['time'] = Clock.get_boottime()
        s['resolution'] = list(map(float, self.size))
        s.ask_update()

    def on_fs(self, instance, value):
        self.canvas.shader.fs = value

    def on_vs(self, instance, value):
        self.canvas.shader.vs = value

Factory.register('ShaderViewer', cls=ShaderViewer)


class ShaderEditor(FloatLayout):

    source = StringProperty('data/logo/kivy-icon-512.png')

    fs = StringProperty('''
void main (void){
    gl_FragColor = frag_color * texture2D(texture0, tex_coord0);
}
''')
    vs = StringProperty('''
void main (void) {
  frag_color = color;
  tex_coord0 = vTexCoords0;
  gl_Position = projection_mat * modelview_mat * vec4(vPosition.xy, 0.0, 1.0);
}
''')

    viewer = ObjectProperty(None)

    def __init__(self, **kwargs):
        super(ShaderEditor, self).__init__(**kwargs)
        self.test_canvas = RenderContext()
        s = self.test_canvas.shader
        self.trigger_compile = Clock.create_trigger(self.compile_shaders, -1)
        self.bind(fs=self.trigger_compile, vs=self.trigger_compile)

    def compile_shaders(self, *largs):
        print('try compile')
        if not self.viewer:
            return

        # we don't use str() here because it will crash with non-ascii char
        if PY2:
            fs = fs_header + self.fs.encode('utf-8')
            vs = vs_header + self.vs.encode('utf-8')
        else:
            fs = fs_header + self.fs
            vs = vs_header + self.vs

        print('-->', fs)
        self.viewer.fs = fs
        print('-->', vs)
        self.viewer.vs = vs


class ShaderEditorApp(App):
    def build(self):
        kwargs = {}
        if len(sys.argv) > 1:
            kwargs['source'] = sys.argv[1]
        return ShaderEditor(**kwargs)

if __name__ == '__main__':
    ShaderEditorApp().run()


demo/shadereditor/shadereditor.kv ファイル
------------------------------------------------------

.. code-block:: python
#:kivy 1.0
#: import GLShaderLexer pygments.lexers.GLShaderLexer

<ShaderEditor>:
    viewer: viewer

    BoxLayout:
        BoxLayout:
            orientation: 'vertical'
            size_hint_x: None
            width: 350

            Label:
                text: 'Fragment Shader'
                size_hint_y: None
                height: self.texture_size[1] + 10
            CodeInput:
                text: root.fs
                lexer: GLShaderLexer()
                on_text: root.fs = args[1]

            Label:
                text: 'Vertex Shader'
                size_hint_y: None
                height: self.texture_size[1] + 10
            CodeInput:
                text: root.vs
                lexer: GLShaderLexer()
                on_text: root.vs = args[1]

        ShaderViewer:
            id: viewer
            canvas:
                Color:
                    rgb: 1, 1, 1
                Rectangle:
                    size: self.size
                    pos: self.pos
                    source: root.source