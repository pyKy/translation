.. 翻訳者: Daisuke Saito

-----------
イベント
-----------

.. Kivy is mostly event-based, meaning the flow of the program is determined by events.
Kivyはイベントベースで、プログラムの流れを意味するイベントによって決定されます。


クロックイベント
-----------------

../_images/gs-events-clock.png

.. The Clock object allows you to schedule a function call in the future as a one-time event with schedule_once(), or as a repetitive event with schedule_interval().
クロックオブジェクトは、schedule_once()に将来的に一回限りのイベントをschedule_interval()に繰り返し行うイベントをスケジュールすることができます。

.. You can also create Triggered events with create_trigger(). Triggers have the advantage of being called only once per frame, even if you have scheduled multiple triggers for the same callback.
create_trigger()を用いてトリガーイベントを作ることができます。トリガーは、同じコールバックトリガーを複数スケジュールした場合でも各フレームごとに一度、コールされるという利点を有しています。


入力イベント
----------------

../_images/gs-events-input.png

.. All the mouse click, touch and scroll wheel events are part of the MotionEvent, extended by Input Postprocessing and dispatched through the on_motion event in the Window class. This event then generates the on_touch_down(), on_touch_move() and on_touch_up() events in the Widget.
すべてのマウスクリック、タッチとスクロールはMotionEventの一部で、Input Postprocessingによって拡張され、Window クラスの中でon_motion()イベントを通じて送られます。
これらのイベントはWidgetにてon_touch_down()on_touch_move()とon_touch_up()イベントを生み出します。

.. For an in-depth explanation, have a look at Input management.
詳細な説明については、入力管理を見ます。

クラスイベント
------------------

../_images/gs-events-class.png

.. Our base class EventDispatcher, used by Widget, uses the power of our Properties for dispatching changes. This means when a widget changes its position or size, the corresponding event is automatically fired.
Widgetにより用いられるベースクラスのEventDispatcherは変更を送るために、プロパティを使います。
これは、Widgetの位置やサイズを変更したときに、対応するイベントが自動的に発生します。

.. In addition, you have the ability to create your own events using register_event_type(), as the on_press and on_release events in the Button widget demonstrate.
加えて、Button widget内のon_pressとon_releaseのイベントが示すように、register_event_type()を使用して、独自のイベントを作成する機能を持っています。

.. Another thing to note is that if you override an event,
.. you become responsible for implementing all its behavior previously handled by the base class. The easiest way to do this is to call super():
注意すべきもう一つのこととして、イベントを上書きした場合は、ベースクラスで以前処理されたビへイビアを実装しなければなりません。
これを行う簡単な方法としてsuper()を呼び出すことです。

.. code-block:python
    def on_touch_down(self, touch):
        if super(OurClassName, self).on_touch_down(touch):
            return True
        if not self.collide_point(touch.x, touch.y):
            return False
        print('you touched me!')
        return True

..Get more familiar with events by reading the Events and Properties documentation.
イベントとプロパティのドキュメントを読むことで、イベントに慣れましょう。

