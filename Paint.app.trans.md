#チュートリアル
## シンプルなペイントアプリケーション

このチュートリアルでは、あなたが、最初にウィジェットを作成数る方法を教えています。
Kivyアプリケーションをプログラミングする際、あなたが特定の目的ためにカスタム要素で
新しいユーザーインターフェースを作成するための最も重要な知識を提供します。

## 基本的な考慮事項
アプリケーションを作成する前に、自分自身で重要な3つの項目を確かめる必要があります。
・私のアプリケーション・プロセスはどのようなデータですか？
・どのように、私は視覚的にそのデータを表しますか？
・どのようにユーザーがそのデータと対話しますか？

たとえば、あなたが非常に単純な線画アプリケーションを記述したいのであれば、
ユーザーは指でスクリーンに描画することをたぶん望みます。


### 翻訳中

```
from kivy.app import App
from kivy.uix.widget import Widget


class MyPaintWidget(Widget):
    pass


class MyPaintApp(App):
    def build(self):
        return MyPaintWidget()


if __name__ == '__main__':
    MyPaintApp().run()
    ```

### 翻訳中

```
from kivy.app import App
from kivy.uix.widget import Widget


class MyPaintWidget(Widget):
    def on_touch_down(self, touch):
        print(touch)


class MyPaintApp(App):
    def build(self):
        return MyPaintWidget()


if __name__ == '__main__':
    MyPaintApp().run()
    ```

