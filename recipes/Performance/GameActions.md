## Goal

Within this recipe you will learn how to use GameActions in Wave Engine games to define a flow of actions you want them to be executed on a given order, but you do not want to worry about the synchronization of these actions with the game loop.

This is very usefull for controlling animations begin, end, and continue with.

GameActions can only be runned in the context of a scene.

## Hands-on

### Flow Example

You Can create different actions from the `GameActionFactory`
You need the a scene to run a game action:

In the next example, when run, **first** execute MyAction, **when finished** continues with second action, then **waits for tap event**, then **waits five more seconds** and them runs SomeAction and **just finish** leaving SomeAction running in background.

You can see and example in [Navigation Service Sample](https://github.com/WaveEngine/Samples/blob/19b2d516170730181a7cc2ca86f753f617ae6a17/Basic/NavigationFlow/SharedSource/Main/Navigation/NavigationService.cs)

```C#
using WaveEngine.Components.GameActions;
...

IGameAction myGameAction = GameActionFactory.CreateEmptyGameAction(this,this.MyAction)
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

## Animations

### Scale animation
For example a simple way of making scale animations for 3D elements is by using `Vector3AnimationGameAction`
```C#
 var amination = new Vector3AnimationGameAction(this.Owner, Vector3.Zero, Vector3.One, TimeSpan.FromSeconds(1), ease, (f) =>
                {
                    this.transform.LocalScale = f;
                });
				
	animation.Run();
```

### Parallel Animations
We can  run *several actions in parallel* and decided when continue, for example a Fade animation with scale that only waits fade and plays sound while scale is still running

```C#
private Transform2D transform = null;
...

	var scale = new Vector2AnimationGameAction(this.Owner, Vector2.Zero, Vector2.One, TimeSpan.FromSeconds(1), ease, (f) =>
                {
                    this.transform.LocalScale = f;
                });
				
	var fade = new Vector2AnimationGameAction(this.Owner, Vector2.Zero, Vector2.One, TimeSpan.FromSeconds(2), ease, (f) =>
	{
		this.transform.LocalOpacity = f;
	});
	
	var sound = GameActionFactory.CreatePlaySoundGameAction(this,appearSound);
				
	var animation = GameActionFactory.CreateParallelGameActions(this, new List<IGameAction>() 
					{
						scale,
						fade
					})
				.WaitAny()
				.ContinueWith(sound);
	
	animation.Run();
```

### And play Animations
Run *parallel* play media for an action, for example an scale and sound effect

```C#
private Transform2D transform = null;
...

	var scale = new Vector2AnimationGameAction(this.Owner, Vector2.Zero, Vector2.One, TimeSpan.FromSeconds(1), ease, (f) =>
                {
                    this.transform.LocalScale = f;
                });
				
	
				
	var animation = GameActionFactory.CreateGameAction(this, scale)
				.AndPlaySound(appearSound)
				.WaitAll();
	
	animation.Run();
```
## Extensions Methods
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
|**AndPlayMusic**| parallel plays music while doing precedent action|
|**AndPlaySound**| parallel plays sound while doing precedent action|
|**AndPlayVideo**| parallel plays video while doing precedent action|

## Wrap-up

You have learned how to use GameActions in WaveEngine to define a flow of actions without having to worry about synchronizations with the game loop.