## Adding network code to your game

Using the `katasdk`, you can easily add network code to your game.

This is possible because the SDK defines several classes that use the regular event system and can *push forward* and *fetch* network messages in an **asynchronous manner**.

## A first example: the game of chess

`katasdk.AsyncMessageCtrl`

## Kata.Games canonical data storage

### registering your game

So your game can push and send data to the server.
The game data gets persisted on the server-side and can be shared amongst many clients.


### synchronization

Synchronize the game model as soon as the remote data has changed (under the hood, the `katasdk` uses a technique named long polling).

