## Goal

Within this recipe you will learn how to use GameActions in Wave Engine games to define a flow of actions you want them to be executed on a given order, but you do not want to worry about the synchronization of these actions with the game loop.

GameActions can only be runned in the context of a scene.

## Hands-on

### With Visual Studio/Xamarin Studio

A typical game action could be:

```C#
IGameAction myGameAction = this.CreateGameAction(this.MyAction)
                               .ContinueWith(this.SecondAction)
                               .AndWaitTap(this.touchGesturesComponent)
                               .Delay(TimeSpan.FromSeconds(5))
                               .ContinueWithAction(() => this.SomeAction());

myGameAction.Run();

...

private IGameAction MyAction(){}

private IGameAction SecondAction(){}

private void SomeAction(){}
```

They are defined as extensions method, and you need to add the namespace *[WaveEngine.Components.GameActions](xref:WaveEngine.Components.GameActions)*

Once you add that namespace, you can use the following set of predefined actions:

|  **Method**  	|**Description**  	| 
|---	|---	|
|**ContinueWith** | continues with a IGameAction|
|**ContinueWithAction**| continues with a .NET action|
|**Delay**| adds a delay to the action flow|
|**AndWaitTap**| stops the flow until a touch tap is detected|
|**CreateEmptyGameAction**| creates an action that does nothing|
|**CreateGameAction**| creates a new IGameAction|
|**CreateGameActionFromAction**| creates a new IGameAction from a .NET action|
|**CreateWaitGameAction**| creates an IGameAction that will wait a given period of time|
|**CreateWaitConditionGameAction**| creates an IGameAction that will stop the flow until the condition is satisfied|
|**CreateSingleAnimationGameAction**| creates an IGameAction to play a SingleAnimation|
|**CreateWaitTapGameAction**| creates an IGameAction that stops the flow until a touch tap is detected|
|**CreateParallelGameActions**| creates an IGameActionSet that will execute the given actions in parallel|
|**CreateLoopGameActionUntil**| creates an IGameAction with a loop that will stop when the stop condition is satisfied|
|**AsSkippableGameAction**| creates an IGameAction that can be skipped|

## Wrap-up

You have learned how to use GameActions in WaveEngine to define a flow of actions without having to worry about synchronizations with the game loop.
