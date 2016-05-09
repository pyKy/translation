Kivy provides a design language specifically geared towards easy and scalable GUI Design. 
Kivyは、簡単で拡張性のあるGUI設計のためのデザイン言語を提供します。

The language makes it simple to separate the interface design from the application logic, 
adhering to the separation of concerns principle. 

言語はインターフェース設計をアプリケーションロジックから切り離すことを簡単にします。
そして、懸念主義の分離を固守します。

For example:
例えば：
https://kivy.org/docs/_images/gs-lang.png

上記のコードは
<LoginScreen>:  # every class in your app can be represented by a rule like
                # this in the kv file
    GridLayout: # this is how you add your widget/layout to the parent
                # (note the indentation).
        rows: 2 # this how you set each property of your widget/layout
        
このように、Kv言語でGUIを設計するのはとても簡単です。より詳細な理解のために、Kv言語ドキュメンテーションをご参照ください。