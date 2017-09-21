## Goal

Within this recipe you will learn how to use the TaskScheduler **Wave Engine Service**.

## Hands-on

Take a look at the [Loading sample](https://github.com/WaveEngine/Samples/blob/master/Performance/Loading/SharedSource/Main/LoadingScene.cs)

You will find in the *Start* method from _LoadingScene_ class:

```C#
WaveServices.TaskScheduler.CreateTask(() => 
{ 
    gameScene = new GameScene(); 
    gameScene.Initialize(WaveServices.GraphicsDevice);
    Thread.Sleep(5000); 
}) 
.ContinueWith(() => 
{ 
    EndAnimation(); 
}); 
```

This is creating a task to initialize the game scene, and sleep the thread five seconds, when initialization task ends, the _EndAnimation_ method will be executed in other thread.

## Wrap-up

You have learn how to make multithreading in your games with some simple lines of code.
