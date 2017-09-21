## Goal

Within this recipe you will learn how to create and manage timers in WaveEngine using built-in WaveServices. A timer use to be defined by a Name, Interval, Action and Loop properties.

* `Name` (_string_): Timer Name, unique in timer collection.
* `Interval` (_timespan_): Time in 'Timespan' format to launch a timer tick.
* `Action` (_action method_): The code to be run on timer tick.
* `Loop` (_boolean_): Indicates when the timer will repeat the action.

## Hands-on

### With Visual Studio/Xamarin Studio

There are some ways to **create** a timer:

```c#
// Interval + Action
var a = WaveServices.TimerFactory.CreateTimer(TimeSpan.FromSeconds(1), () => {});

// Name + Interval + Action
var b = WaveServices.TimerFactory.CreateTimer("timerName", TimeSpan.FromSeconds(1), () => {});

// Interval + Action + Loop
var c = WaveServices.TimerFactory.CreateTimer(TimeSpan.FromSeconds(1), () => {}, true);

// Name + Interval + Action + Loop
var d = WaveServices.TimerFactory.CreateTimer("timerName2", TimeSpan.FromSeconds(1), () => { }, true);
```

You can **manage** a timer state with Pause and Resume methods:

```c#
var timer = WaveServices.TimerFactory.CreateTimer(TimeSpan.FromSeconds(1), () => { });
timer.Pause();
timer.Resume();
```

No-looped timers are destroyed after fired so you don't need to remove them, but looped timers must be removed from scene to avoid strange behaviors. 
You can **remove** timers by name, using the same timer object or remove all timers from the game or scene:

```c#
// Remove a timer by name or timer object
WaveServices.TimerFactory.RemoveTimer("timerName");
WaveServices.TimerFactory.RemoveTimer(timer);

// Remove timer from a scene object
WaveServices.TimerFactory.RemoveSceneTimers(scene);

// Remove timer from this scene (this code only works on a Scene class)
WaveServices.TimerFactory.RemoveSceneTimers(this);

// Remove All game timers
WaveServices.TimerFactory.RemoveAllTimers();
```

## Wrap-up

Within this recipe we have created some timers, managed timers states by Pause/Resume methods, and removed the timers by different ways.