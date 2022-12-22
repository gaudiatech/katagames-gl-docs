# What is kengi?

`kengi` denotes a custom game engine built in France between year 2018 and year 2022; `kengi` is used throughout all game that are hosted via [Kata.Games](https://kata.games).

Note that our specific engine is fully written in python.

One very feature of `kengi` is that it mimics
the API of the popular `pygame` library.
The benefit is that if you have some prior experience with creating games using `pygame`,
transitioning to `kengi` should be smooth and easy.


## A primer on writing games

TODO::

simple game

Creating simple games is as easy as sub-classing the `kengi.GameTpl` abstract class.
You will need to redefine the `.enter(vms=None)` method of this class.

In case you need to add GUI elements to your game,
check out the [dedicated page](gui).

## Creating more complex games

Most game have several "modes", that is you don't interact with the software in the same way,
from start to finish.
For example if you play a tetris clone, there could be (a) A title screen, (b) the game screen and (c) the Highscores screen.

!!! tip
	If you're creating a complex game with `kengi`,
	you simply define and declare and your game states before using them.
	To declare gamestates, one can use a special function.
	It's recommended to set the alias: `enum_builder = kengi.struct.enum` then use the enum_builder like any function,
	with `n` parameters.

* to define a gamestate you need to add a sub-class of the `kengi.BaseGameState` class.
* then, in this new class you'd need to redefine at least methods `.enter()` and `.release()`. You can also redefine methodes `.pause()` and `.resume()` if you feel it's useful to pause the execution of a gamestate to resume it later while keeping the exact same data!
* once your classes are ready, list all your gamestate. Each gamestate has a name, it's a `str`
* you will need to call `kengi.declare_states(the_enum, {stcode_k: state_obj_k...})`. Once you've done this, the GameTpl class will be automatically upgraded.

to change state:
you would use:
either `StateChange` or `StatePush` (both contain a `state_ident` attribute!) or just `StatePop` event.
All these events are pre-defined in`kengi.EngineEvTypes`.

This page source file and media content have been localized after applying
the [localized build logic](#localized-build-logic) described below.


## A Basic Example

Here is a quick recap of the files used as source and the generated build structure of
what you see:

```python
docs
├── image.en.png  <-- this image file is used here
├── image.fr.png
├── index.fr.md
├── index.md  <-- this file is used here
├── topic1
│   ├── named_file.en.md
│   └── named_file.fr.md
└── topic2
    ├── index.en.md
    └── index.md
```

```
site
├── en
│   ├── image.png  <-- you see this image here on the /en version
│   ├── index.html  <-- you are here on the /en version
│   ├── topic1
│   │   └── named_file
│   │       └── index.html
│   └── topic2
│       └── index.html
├── fr
│   ├── image.png
│   ├── index.html
│   ├── topic1
│   │   └── named_file
│   │       └── index.html
│   └── topic2
│       └── index.html
├── image.png  <-- you see this image here on the default version
├── index.html  <-- you are here on the default version
├── topic1
│   └── named_file
│       └── index.html
└── topic2
    └── index.html
```


## Automatic media / link / asset localization

![localized image](image.png)

This image source is dynamically localized while still being referenced in the
markdown source of the page as `![localized image](image.png)`. This means that
this plugin allows you to not worry about links, media and static content file
names, just use their simple name and concentrate on your content translation!


## Localized build logic

The settings used to build this site is:

```
plugins:
  - i18n:
      default_language: en
      languages:
        en: english
        fr: français
```

Meaning that we will get three versions of our website:

1. the `default_language` version which will use any `.md` documentation file first and fallback to any `.en.md` file found since `en` is the default language
2. the `/en` language version which will use any `.en.md` documentation file first and fallback to any `.md` file found
3. the `/fr` language version which will use any `.fr.md` documentation file first and fallback to either `.en.md` file (default language) or `.md` file (default language fallback) whichever comes first

Given that logic, the following `site` structure is built:

```
site
├── 404.html
├── assets
│   ├── images
│   ├── javascripts
│   └── stylesheets
├── en
│   ├── image.png
│   ├── index.html
│   ├── topic1
│   │   └── named_file
│   │       └── index.html
│   └── topic2
│       └── index.html
├── fr
│   ├── image.png
│   ├── index.html
│   ├── topic1
│   │   └── named_file
│   │       └── index.html
│   └── topic2
│       └── index.html
├── image.png
├── index.html
├── topic1
│   └── named_file
│       └── index.html
└── topic2
    └── index.html
```
