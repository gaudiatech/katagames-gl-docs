# Building the GUI

GUI or G.U.I. denotes the **Graphical User Interface**.

Sometimes you could need a GUI to implement your game properly.
This is true, especially for game genres that use the mouse as the main interaction/input method (strategy games, RPGs, *etc.*)
The simplest way to build your GUI is to add a set of labels and buttons.

## Adding labels

To add labels, one creates objects like that:
```python
    def __init__(self):
        super().__init__()

        self.title = kengi.gui.Label(
			(sw//2, 100),
			'The game of chess',
			anchoring=kengi.gui.ANCHOR_CENTER
		)
        self.title.textsize = 122
        self.title.color = 'brown'
```

There are several options that can be passed to the `kengi.gui.Label` constructor:
```python
(position, text, txtsize=35, color=None, anchoring=ANCHOR_LEFT, debugmode=False)
```
Also, the `textsize`, the `color` and the `text` can be changed dynamically.

Displaying a label is *super easy*, all you need to do is calling the `.draw()` method for every label that you have.
```python
    def on_paint(self, ev):
        # ...refresh the screen...

        self.my_label.draw()
```


## Adding buttons

To add buttons,
in the constructor of one of your classes, you could write:

```python
	def __init__(self):
		super().__init__()
		self.my_button = kengi.gui.Button((128, 256), (200, 50), 'Start Chessmatch')
		self.my_button.callback = proc_button
```
the `proc_button` function needs to be defined. This will determine what's the effect
associated to pressing the `my_button` object.

!!! tip
	The `kengi.gui.Button` class inherits from `pygame.sprite.Sprite`. If you're already familiar
	with the interface of `Sprite` it will be easy to use buttons.
	In particular, every `Sprite` has a `.rect` and a `.image` attribute.
	These are used to locate your button and to draw it, just like for any regular `Sprite` object.

Once your button is instanciated you will need to display it. The recommended way to do that is:

```python
    def on_paint(self, ev):
        # ...refresh the screen...

        ev.screen.blit(
            self.my_button.image,
            self.my_button.rect.topleft
        )
```

Then, you need to manually handle the click detection.
In the class that models your view/game component you can proceed as follows:

```python
	def on_mousedown(self, ev):
		if self.my_button.rect.collidepoint(ev.pos):
			self.my_button.callback()
```

Most of times, the `proc_button` function is a very basic operation that sends something useful to the event manager.

For example, if you are creating a RPG-like game, this would be useful to open the inventory without loosing the data of the current game state (maybe called `MyStates.Exploration`).

```
def proc_button():
    kengi.get_ev_manager().post(
		EngineEvTypes.StatePush, state_ident=MyStates.Inventory
	)
```	
