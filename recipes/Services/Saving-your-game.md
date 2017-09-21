## Goal

Within this recipe you will learn how to save your game state to persistent storage. For example, when our game is running in a mobile phone, we need save the game state when the user receives a call, due to the OS of the device can kill the game application if it needs more memory, and the user will want to resume the game in the previous state.

## Hands-on

### With Visual Studio/Xamarin Studio

WaveEngine comes with an storage service out of the box. This service manages the serialization and persistence, allowing us, for example, to store the game state. The service is based on the [DataContract](https://msdn.microsoft.com/es-es/library/system.runtime.serialization.datacontractserializer(v=vs.110).aspx) model of .Net Framework.

To use it, you should declare the class which contains the state of our game.

```C#
 public class GameState
 {
     ...
     public Vector2 PlayerPosition;
     ...
 }
```

Now, we need to register the class and other ones used by such, in the storage service.

For example, we can do such in our game's `Game.Initialize()`.

```C#
 protected override void Initialize()
 {
    ...
    this.storageService = WaveServices.Storage;
    this.storageService.SetKnownTypes(new[] { typeof(GameState) });
	...
 }
```

Finally, we can write and read the game state, for instance, inside the lifecycle methods of our game class:

```C#
 protected override void OnActivated()
 {
    base.OnActivated();
    ...
    if (this.storageService.Exists("GameState"))
    {
        this.gameState = this.storageService.Read<GameState>("GameState");
        this.storageService.Delete("GameState");
    }
	...
 }
```
```C#
 protected override void OnDeactivated()
 {
    base.OnDeactivated();
    ...
    this.storageService.Write<GameState>("GameState", this.gameState);
	...
 }
```

## Wrap-up

We have seen how to store and read the game state of our game. Using this service we can back-up such in the persistent storage.