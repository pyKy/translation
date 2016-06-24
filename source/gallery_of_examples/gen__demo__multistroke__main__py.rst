.. 翻訳者: Jun Okazaki

------------------------------------------------------------------------
Multistroke Recognition Database Demonstration
------------------------------------------------------------------------

このドキュメンテーションはGallery of Examples » Multistroke Recognition Database Demonstrationを日本語訳したものです。  
https://kivy.org/docs/examples/gen__demo__multistroke__main__py.html

.. image:: https://kivy.org/docs/_images/demo__multistroke__main__py1.png

このアプリケーションは、ジェスチャーを記録し、一致させようとします。
黒色の描画面と下部分にいくつかのボタンとが表示されます。
描画面上でジェスチャーを行うと、ジェスチャーが履歴に追加され、一致が試みられます。
履歴タブに移動しジェスチャーに名前を付けてデータベースに追加した場合、将来的に同様のジェスチャーが認識されます。
.kgファイルのジェスチャーのデータベースにロードと保存ができます。

デモのコードは、プライマリファイルであることで、多くのファイルにまたがっています。
ポップアップ情報は、（「いいえマッチ '）ファイルhelpers.pyから来ています。

履歴ペインは、historymanager.pyファイルで管理され、historymanager.kvファイルに記載されています。
データベースペインとストレージは、gestureDatabase.pyファイルで管理され、gestureDatabase.kvファイルに記載されています。
スライダーやボタンの一般的なロジックは、settings.pyファイルにあり、settings.kvで説明します。
実際の設定ペインは、multistroke.kvファイルに記載されており、このファイルから管理されています。


demo/multistroke/main.py ファイル
------------------------------------

.. code-block:: python
'''
Multistroke Recognition Database Demonstration
==============================================

This application records gestures and attempts to match them. You should
see a black drawing surface with some buttons across the bottom. As you
make a gesture on the drawing surface, the gesture will be added to
the history and a match will be attempted. If you go to the history tab,
name the gesture, and add it to the database, then simliar gestures in the
future will be recognized. You can load and save databases of gestures
in .kg files.

This demonstration code spans many files, with this being the primary file.
The information pop-up ('No match') comes from the file helpers.py.
The history pane is managed in the file historymanager.py and described
in the file historymanager.kv. The database pane and storage is managed in
the file gestureDatabase.py and the described in the file gestureDatabase.kv.
The general logic of the sliders and buttons are in the file
settings.py and described in settings.kv. but the actual settings pane is
described in the file multistroke.kv and managed from this file.

'''
from kivy.app import App
from kivy.uix.gridlayout import GridLayout
from kivy.uix.gesturesurface import GestureSurface
from kivy.uix.screenmanager import ScreenManager, Screen, SlideTransition
from kivy.uix.label import Label
from kivy.multistroke import Recognizer

# Local libraries
from historymanager import GestureHistoryManager
from gesturedatabase import GestureDatabase
from settings import MultistrokeSettingsContainer


class MainMenu(GridLayout):
    pass


class MultistrokeAppSettings(MultistrokeSettingsContainer):
    pass


class MultistrokeApp(App):

    def goto_database_screen(self, *l):
        self.database.import_gdb()
        self.manager.current = 'database'

    def handle_gesture_cleanup(self, surface, g, *l):
        if hasattr(g, '_result_label'):
            surface.remove_widget(g._result_label)

    def handle_gesture_discard(self, surface, g, *l):
        # Don't bother creating Label if it's not going to be drawn
        if surface.draw_timeout == 0:
            return

        text = '[b]Discarded:[/b] Not enough input'
        g._result_label = Label(text=text, markup=True, size_hint=(None, None),
                                center=(g.bbox['minx'], g.bbox['miny']))
        self.surface.add_widget(g._result_label)

    def handle_gesture_complete(self, surface, g, *l):
        result = self.recognizer.recognize(g.get_vectors())
        result._gesture_obj = g
        result.bind(on_complete=self.handle_recognize_complete)

    def handle_recognize_complete(self, result, *l):
        self.history.add_recognizer_result(result)

        # Don't bother creating Label if it's not going to be drawn
        if self.surface.draw_timeout == 0:
            return

        best = result.best
        if best['name'] is None:
            text = '[b]No match[/b]'
        else:
            text = 'Name: [b]%s[/b]\nScore: [b]%f[/b]\nDistance: [b]%f[/b]' % (
                   best['name'], best['score'], best['dist'])

        g = result._gesture_obj
        g._result_label = Label(text=text, markup=True, size_hint=(None, None),
                                center=(g.bbox['minx'], g.bbox['miny']))
        self.surface.add_widget(g._result_label)

    def build(self):
        # Setting NoTransition breaks the "history" screen! Possibly related
        # to some inexplicable rendering bugs on my particular system
        self.manager = ScreenManager(transition=SlideTransition(
                                     duration=.15))
        self.recognizer = Recognizer()

        # Setup the GestureSurface and bindings to our Recognizer
        surface = GestureSurface(line_width=2, draw_bbox=True,
                                 use_random_color=True)
        surface_screen = Screen(name='surface')
        surface_screen.add_widget(surface)
        self.manager.add_widget(surface_screen)

        surface.bind(on_gesture_discard=self.handle_gesture_discard)
        surface.bind(on_gesture_complete=self.handle_gesture_complete)
        surface.bind(on_gesture_cleanup=self.handle_gesture_cleanup)
        self.surface = surface

        # History is the list of gestures drawn on the surface
        history = GestureHistoryManager()
        history_screen = Screen(name='history')
        history_screen.add_widget(history)
        self.history = history
        self.manager.add_widget(history_screen)

        # Database is the list of gesture templates in Recognizer
        database = GestureDatabase(recognizer=self.recognizer)
        database_screen = Screen(name='database')
        database_screen.add_widget(database)
        self.database = database
        self.manager.add_widget(database_screen)

        # Settings screen
        app_settings = MultistrokeAppSettings()
        ids = app_settings.ids

        ids.max_strokes.bind(value=surface.setter('max_strokes'))
        ids.temporal_win.bind(value=surface.setter('temporal_window'))
        ids.timeout.bind(value=surface.setter('draw_timeout'))
        ids.line_width.bind(value=surface.setter('line_width'))
        ids.draw_bbox.bind(value=surface.setter('draw_bbox'))
        ids.use_random_color.bind(value=surface.setter('use_random_color'))

        settings_screen = Screen(name='settings')
        settings_screen.add_widget(app_settings)
        self.manager.add_widget(settings_screen)

        # Wrap in a gridlayout so the main menu is always visible
        layout = GridLayout(cols=1)
        layout.add_widget(self.manager)
        layout.add_widget(MainMenu())
        return layout


if __name__ in ('__main__', '__android__'):
    MultistrokeApp().run()

demo/multistroke/helpers.py ファイル
------------------------------------------------------

.. code-block:: python
__all__ = ('InformationPopup', )

from kivy.uix.popup import Popup
from kivy.properties import StringProperty
from kivy.factory import Factory
from kivy.lang import Builder
from kivy.clock import Clock

Builder.load_string('''
<InformationPopup>:
    auto_dismiss: True
    size_hint: None, None
    size: 400, 200
    on_open: root.dismiss_trigger()
    title: root.title
    Label:
        text: root.text
''')


class InformationPopup(Popup):
    title = StringProperty('Information')
    text = StringProperty('')

    def __init__(self, time=1.5, **kwargs):
        super(InformationPopup, self).__init__(**kwargs)
        self.dismiss_trigger = Clock.create_trigger(self.dismiss, time)


Factory.register('InformationPopup', cls=InformationPopup)

demo/multistroke/historymanager.py ファイル
------------------------------------------------------

.. code-block:: python
__all__ = ('GestureHistoryManager', 'GestureVisualizer')

from kivy.app import App
from kivy.clock import Clock
from kivy.lang import Builder
from kivy.uix.widget import Widget
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.gridlayout import GridLayout
from kivy.uix.label import Label
from kivy.graphics import Color, Line
from kivy.properties import ObjectProperty, BooleanProperty
from kivy.compat import PY2

# local libraries
from helpers import InformationPopup
from settings import MultistrokeSettingsContainer


# refuse heap permute for gestures with more strokes than 3
# (you can increase it, but 4 strokes = 384 templates, 5 = 3840)
MAX_PERMUTE_STROKES = 3

Builder.load_file('historymanager.kv')


class GestureHistoryManager(GridLayout):
    selected = ObjectProperty(None, allownone=True)

    def __init__(self, **kwargs):
        super(GestureHistoryManager, self).__init__(**kwargs)
        self.gesturesettingsform = GestureSettingsForm()
        rr = self.gesturesettingsform.rrdetails
        rr.bind(on_reanalyze_selected=self.reanalyze_selected)
        self.infopopup = InformationPopup()
        self.recognizer = App.get_running_app().recognizer

    def reanalyze_selected(self, *l):
        # recognize() can block the UI with max_gpf=100, show a message
        self.infopopup.text = 'Please wait, analyzing ..'
        self.infopopup.auto_dismiss = False
        self.infopopup.open()

        # Get a reference to the original GestureContainer object
        gesture_obj = self.selected._result_obj._gesture_obj

        # Reanalyze the candidate strokes using current database
        res = self.recognizer.recognize(gesture_obj.get_vectors(),
                                        max_gpf=100)

        # Tag the result with the gesture object (it didn't change)
        res._gesture_obj = gesture_obj

        # Tag the selected item with the updated ProgressTracker
        self.selected._result_obj = res
        res.bind(on_complete=self._reanalyze_complete)

    def _reanalyze_complete(self, *l):
        self.gesturesettingsform.load_visualizer(self.selected)
        self.infopopup.dismiss()

    def add_selected_to_database(self, *l):
        if self.selected is None:
            raise Exception('add_gesture_to_database before load_visualizer?')

        if self.gesturesettingsform.addsettings is None:
            raise Exception('add_gesture_to_database missing addsetings?')

        ids = self.gesturesettingsform.addsettings.ids

        name = ids.name.value.strip()
        if name == '':
            self.infopopup.auto_dismiss = True
            self.infopopup.text = 'You must specify a name for the gesture'
            self.infopopup.open()
            return

        permute = ids.permute.value
        sensitive = ids.orientation_sens.value
        strokelen = ids.stroke_sens.value
        angle_sim = ids.angle_sim.value

        cand = self.selected._result_obj._gesture_obj.get_vectors()

        if permute and len(cand) > MAX_PERMUTE_STROKES:
            t = "Can't heap permute %d-stroke gesture " % (len(cand))
            self.infopopup.text = t
            self.infopopup.auto_dismiss = True
            self.infopopup.open()
            return

        self.recognizer.add_gesture(
            name,
            cand,
            use_strokelen=strokelen,
            orientation_sensitive=sensitive,
            angle_similarity=angle_sim,
            permute=permute)

        self.infopopup.text = 'Gesture added to database'
        self.infopopup.auto_dismiss = True
        self.infopopup.open()

    def clear_history(self, *l):
        if self.selected:
            self.visualizer_deselect()
        self.ids.history.clear_widgets()

    def visualizer_select(self, visualizer, *l):
        if self.selected is not None:
            self.selected.selected = False
        else:
            self.add_widget(self.gesturesettingsform)

        self.gesturesettingsform.load_visualizer(visualizer)
        self.selected = visualizer

    def visualizer_deselect(self, *l):
        self.selected = None
        self.remove_widget(self.gesturesettingsform)

    def add_recognizer_result(self, result, *l):
        '''The result object is a ProgressTracker with additional
        data; in main.py it is tagged with the original GestureContainer
        that was analyzed (._gesture_obj)'''

        # Create a GestureVisualizer that draws the gesture on canvas
        visualizer = GestureVisualizer(result._gesture_obj,
                                       size_hint=(None, None), size=(150, 150))

        # Tag it with the result object so AddGestureForm.load_visualizer
        # has the results to build labels in the scrollview
        visualizer._result_obj = result

        visualizer.bind(on_select=self.visualizer_select)
        visualizer.bind(on_deselect=self.visualizer_deselect)

        # Add the visualizer to the list of gestures in 'history' screen
        self.ids.history.add_widget(visualizer)
        self._trigger_layout()
        self.ids.scrollview.update_from_scroll()


class RecognizerResultLabel(Label):
    '''This Label subclass is used to show a single result from the
    gesture matching process (is a child of GestureHistoryManager)'''
    pass


class RecognizerResultDetails(BoxLayout):
    '''Contains a ScrollView of RecognizerResultLabels, ie the list of
    matched gestures and their score/distance (is a child of
    GestureHistoryManager)'''
    def __init__(self, **kwargs):
        super(RecognizerResultDetails, self).__init__(**kwargs)
        self.register_event_type('on_reanalyze_selected')

    def on_reanalyze_selected(self, *l):
        pass


class AddGestureSettings(MultistrokeSettingsContainer):
    pass


class GestureSettingsForm(BoxLayout):
    '''This is the main content of the GestureHistoryManager, the form for
    adding a new gesture to the recognizer. It is added to the widget tree
    when a GestureVisualizer is selected.'''

    def __init__(self, **kwargs):
        super(GestureSettingsForm, self).__init__(**kwargs)
        self.infopopup = InformationPopup()
        self.rrdetails = RecognizerResultDetails()
        self.addsettings = None
        self.app = App.get_running_app()

    def load_visualizer(self, visualizer):
        if self.addsettings is None:
            self.addsettings = AddGestureSettings()
            self.ids.settings.add_widget(self.addsettings)

        self.visualizer = visualizer
        analysis = self.ids.analysis
        analysis.clear_widgets()
        analysis.add_widget(self.rrdetails)

        scrollv = self.rrdetails.ids.result_scrollview
        resultlist = self.rrdetails.ids.result_list
        resultlist.clear_widgets()

        r = visualizer._result_obj.results

        if not len(r):
            lbl = RecognizerResultLabel(text='[b]No match[/b]')
            resultlist.add_widget(lbl)
            scrollv.scroll_y = 1
            return

        if PY2:
            d = r.iteritems
        else:
            d = r.items

        for one in sorted(d(), key=lambda x: x[1]['score'],
                          reverse=True):
            data = one[1]
            lbl = RecognizerResultLabel(
                text='Name: [b]' + data['name'] + '[/b]' +
                     '\n      Score: ' + str(data['score']) +
                     '\n      Distance: ' + str(data['dist']))
            resultlist.add_widget(lbl)

        # Make sure the top is visible
        scrollv.scroll_y = 1


class GestureVisualizer(Widget):
    selected = BooleanProperty(False)

    def __init__(self, gesturecontainer, **kwargs):
        super(GestureVisualizer, self).__init__(**kwargs)

        self._gesture_container = gesturecontainer

        self._trigger_draw = Clock.create_trigger(self._draw_item, 0)
        self.bind(pos=self._trigger_draw, size=self._trigger_draw)
        self._trigger_draw()

        self.register_event_type('on_select')
        self.register_event_type('on_deselect')

    def on_touch_down(self, touch):
        if not self.collide_point(touch.x, touch.y):
            return
        self.selected = not self.selected
        self.dispatch(self.selected and 'on_select' or 'on_deselect')

    # FIXME: This seems inefficient, is there a better way??
    def _draw_item(self, dt):
        g = self._gesture_container
        bb = g.bbox
        minx, miny, maxx, maxy = bb['minx'], bb['miny'], bb['maxx'], bb['maxy']
        width, height = self.size
        xpos, ypos = self.pos

        if g.height > g.width:
            to_self = (height * 0.85) / g.height
        else:
            to_self = (width * 0.85) / g.width

        self.canvas.remove_group('gesture')

        cand = g.get_vectors()
        col = g.color
        for stroke in cand:
            out = []
            append = out.append
            for vec in stroke:
                x, y = vec
                x = (x - minx) * to_self
                w = (maxx - minx) * to_self
                append(x + xpos + (width - w) * .85 / 2)

                y = (y - miny) * to_self
                h = (maxy - miny) * to_self
                append(y + ypos + (height - h) * .85 / 2)

            with self.canvas:
                Color(col[0], col[1], col[2], mode='rgb')
                Line(points=out, group='gesture', width=2)

    def on_select(self, *l):
        pass

    def on_deselect(self, *l):
        pass
        

demo/multistroke/historymanager.py ファイル
------------------------------------------------------

.. code-block:: python
<GestureHistoryManager>:
    rows: 1
    spacing: 10
    GridLayout:
        cols: 1
        size_hint_x: None
        width: 150
        canvas:
            Color:
                rgba: 1, 1, 1, .1
            Rectangle:
                size: self.size
                pos: self.pos

        Button:
            text: 'Clear History'
            size_hint_y: None
            height: 50
            on_press: root.clear_history()

        ScrollView:
            id: scrollview
            scroll_type: ['bars', 'content']
            bar_width: 4
            GridLayout:
                id: history
                cols: 1
                size_hint: 1, None
                height: self.minimum_height

<GestureSettingsForm>:
    orientation: 'vertical'
    spacing: 10
    GridLayout:
        id: settings
        cols: 1
        top: root.top
        Label:
            text: '[b]Results (scroll for more)[/b]'
            markup: True
            size_hint_y: None
            height: 30
            halign: 'left'
            valign: 'middle'
            text_size: self.size
            canvas:
                Color:
                    rgba: 47 / 255., 167 / 255., 212 / 255., .4
                Rectangle:
                    pos: self.x, self.y + 1
                    size: self.size
                Color:
                    rgb: .5, .5, .5
                Rectangle:
                    pos: self.x, self.y - 2
                    size: self.width, 1

        GridLayout:
            id: analysis
            top: root.top
            rows: 1

<GestureVisualizer>:
    canvas:
        Color:
            rgba: 1, 1, 1, self.selected and .3 or .1
        Rectangle:
            pos: self.pos
            size: self.size


<RecognizerResultDetails>:
    canvas:
        Color:
            rgba: 1, 0, 0, .1
        Rectangle:
            size: self.size
            pos: self.pos

    ScrollView:
        id: result_scrollview
        scroll_type: ['bars', 'content']
        bar_width: 4
        GridLayout:
            id: result_list
            cols: 1
            size_hint: 1, None
            height: self.minimum_height

    Button:
        size_hint: None, None
        width: 150
        height: 70
        text: 'Re-analyze'
        on_press: root.dispatch('on_reanalyze_selected')


<RecognizerResultLabel>:
    size_hint_y: None
    height: 70
    markup: True
    halign: 'left'
    valign: 'top'
    text_size: self.size


<AddGestureSettings>:
    MultistrokeSettingTitle:
        title: 'New gesture settings'
        desc: 'Affects how to future input is matched against new gesture'

    MultistrokeSettingBoolean:
        id: permute
        title: 'Use Heap Permute algorithm?'
        desc:
            ('This will generate all possible stroke orders from the ' +
            'input. Only suitable for gestures with 1-3 strokes (or ' +
            'the number of templates will be huge)')
        button_text: 'Heap Permute?'
        value: True

    MultistrokeSettingBoolean:
        id: stroke_sens
        title: 'Require same number of strokes?'
        desc:
            ('When enabled, the new gesture will only match candidates ' +
            'with exactly the same stroke count. Enable if possible.')
        button_text: 'Stroke sensitive?'
        value: True

    MultistrokeSettingBoolean:
        id: orientation_sens
        title: 'Is gesture orientation sensitive?'
        desc:
            ('Enable to differentiate gestures that differ only by ' +
            'orientation (d/p, b/q, w/m), disable for gestures that ' +
            'look the same in any orientation (like a circle)')
        button_text: 'Orientation\nsensitive?'
        value: True

    MultistrokeSettingSlider:
        id: angle_sim
        title: 'Angle similarity threshold'
        type: 'float'
        desc:
            ('Use a low number to distinguish similar gestures, higher ' +
            'number to match similar gestures (with differing angle)')
        value: 30.
        min: 1.0
        max: 179.0

    MultistrokeSettingString:
        id: name
        title: 'Gesture name'
        type: 'float'
        desc:
            ('Name of new gesture (including all generated templates). ' +
            'You can have as many gestures with the same name as you need')

    Button:
        size_hint_y: None
        height: 40
        text: 'Add to database'
        on_press: root.parent.parent.parent.add_selected_to_database()


demo/multistroke/gestureDatabase.kv ファイル
------------------------------------------------------


demo/multistroke/settings.py ファイル
------------------------------------------------------

.. code-block:: python
__all__ = ('MultistrokeSettingsContainer', 'MultistrokeSettingItem',
           'MultistrokeSettingBoolean', 'MultistrokeSettingSlider',
           'MultistrokeSettingString', 'MultistrokeSettingTitle')

from kivy.factory import Factory
from kivy.lang import Builder
from kivy.uix.gridlayout import GridLayout
from kivy.uix.label import Label
from kivy.properties import (StringProperty, NumericProperty, OptionProperty,
                             BooleanProperty)
from kivy.uix.popup import Popup

Builder.load_file('settings.kv')


class MultistrokeSettingsContainer(GridLayout):
    pass


class MultistrokeSettingBoolean(MultistrokeSettingItem):
    button_text = StringProperty('')
    value = BooleanProperty(False)


class MultistrokeSettingString(MultistrokeSettingItem):
    value = StringProperty('')


class EditSettingPopup(Popup):
    def __init__(self, **kwargs):
        super(EditSettingPopup, self).__init__(**kwargs)
        self.register_event_type('on_validate')

    def on_validate(self, *l):
        pass


class MultistrokeSettingSlider(MultistrokeSettingItem):
    min = NumericProperty(0)
    max = NumericProperty(100)
    type = OptionProperty('int', options=['float', 'int'])
    value = NumericProperty(0)

    def __init__(self, **kwargs):
        super(MultistrokeSettingSlider, self).__init__(**kwargs)
        self._popup = EditSettingPopup()
        self._popup.bind(on_validate=self._validate)
        self._popup.bind(on_dismiss=self._dismiss)

    def _to_numtype(self, v):
        try:
            if self.type == 'float':
                return round(float(v), 1)
            else:
                return int(v)
        except ValueError:
            return self.min

    def _dismiss(self, *l):
        self._popup.ids.input.focus = False

    def _validate(self, instance, value):
        self._popup.dismiss()
        val = self._to_numtype(self._popup.ids.input.text)
        if val < self.min:
            val = self.min
        elif val > self.max:
            val = self.max
        self.value = val

    def on_touch_down(self, touch):
        if not self.ids.sliderlabel.collide_point(*touch.pos):
            return super(MultistrokeSettingSlider, self).on_touch_down(touch)
        ids = self._popup.ids
        ids.value = str(self.value)
        ids.input.text = str(self._to_numtype(self.value))
        self._popup.open()
        ids.input.focus = True
        ids.input.select_all()


Factory.register('MultistrokeSettingsContainer',
                 cls=MultistrokeSettingsContainer)
Factory.register('MultistrokeSettingTitle', cls=MultistrokeSettingTitle)
Factory.register('MultistrokeSettingBoolean', cls=MultistrokeSettingBoolean)
Factory.register('MultistrokeSettingSlider', cls=MultistrokeSettingSlider)
Factory.register('MultistrokeSettingString', cls=MultistrokeSettingString)


demo/multistroke/multistroke.py ファイル
------------------------------------------------------

.. code-block:: python
<MainMenu>:
    rows: 1
    size_hint: (1, None)
    height: 50
    spacing: 5
    padding: 5
    ToggleButton:
        group: 'mainmenu'
        state: 'down'
        text: 'Gesture Surface'
        on_press:
            app.manager.current = 'surface'
            if self.state == 'normal': self.state = 'down'
    ToggleButton:
        group: 'mainmenu'
        text: 'History'
        on_press:
            app.manager.current = 'history'
            if self.state == 'normal': self.state = 'down'
    ToggleButton:
        group: 'mainmenu'
        text: 'Database'
        on_press:
            app.goto_database_screen()
            if self.state == 'normal': self.state = 'down'
    ToggleButton:
        group: 'mainmenu'
        text: 'Settings'
        on_press:
            app.manager.current = 'settings'
            if self.state == 'normal': self.state = 'down'

<MultistrokeAppSettings>:
    pos_hint: {'top': 1}

    MultistrokeSettingTitle:
        title: 'GestureSurface behavior'
        desc: 'Affects how gestures are detected and cleaned up'

    MultistrokeSettingSlider:
        id: max_strokes
        title: 'Max strokes'
        type: 'int'
        desc:
            ('Max number of strokes for a single gesture. If 0, the ' +
            'gesture will only be analyzed once the temporal window has ' +
            'expired since the last strokes touch up event')
        value: 4
        min: 0
        max: 15

    MultistrokeSettingSlider:
        id: temporal_win
        title: 'Temporal Window'
        type: 'float'
        desc:
            ('Time to wait from last touch up in a gesture before analyzing ' +
            'the input. If 0, only analyzed once Max Strokes is reached')
        value: 2.
        min: 0
        max: 60.

    MultistrokeSettingTitle:
        title: 'Drawing'
        desc: 'Affects how gestures are visualized on the GestureSurface'

    MultistrokeSettingSlider:
        id: timeout
        title: 'Draw Timeout'
        type: 'float'
        desc:
            ('How long to display the gesture (and result label) on the ' +
            'gesture surface once analysis has completed')
        value: 2.
        min: 0
        max: 60.

    MultistrokeSettingSlider:
        id: line_width
        title: 'Line width'
        type: 'int'
        desc:
            ('Width of lines on the gesture surface; 0 does not draw ' +
            'anything; 1 uses OpenGL line, >1 uses custom drawing method.')
        value: 2
        min: 0
        max: 10

    MultistrokeSettingBoolean:
        id: use_random_color
        title: 'Use random color?'
        desc: 'Use random color for each gesture? If disabled, white is used.'
        button_text: 'Random color?'
        value: True

    MultistrokeSettingBoolean:
        id: draw_bbox
        title: 'Draw gesture bounding box?'
        desc: 'Enable to draw a bounding box around the gesture'
        button_text: 'Draw bbox?'
        value: True
