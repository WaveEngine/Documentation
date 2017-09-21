## Goal

An screen transition is an action which moves the app from the current screen to a different one, applying a custom animation.

[![Screen transitions in Wave Engine](http://img.youtube.com/vi/mzny7KsJhRA/0.jpg)](https://youtu.be/mzny7KsJhRA)

As you can see above, Wave Engine includes a comprehensive set of screen transitions (sample source code can be found [here](https://github.com/WaveEngine/Samples/tree/master/Core/TransitionSample)).

## Hands-on

> [!Note] 
> Screen transitions cannot currently be handled from Wave Visual Editor, so we will concentrate on the IDEs.

Transitions are managed by [ScreenContext](xref:WaveEngine.Framework.Services.ScreenContext) class. It is a way of easily grouping scenes, i.e. it makes send to have a "view" made of two different scenes: an animated 3D background and a 2D menu on top of it. You need to create a new `ScreenContext` to navigate to, and apply the `ScreenTransition` that Wave Engine offers (click [here](xref:WaveEngine.Framework.Services.ScreenTransition) to see those built in).

The following piece of code shows up how to navigate to a `SecondScene` on a [ColorFadeTransition](xref:WaveEngine.Components.Transitions.ColorFadeTransition), which needs the background color to fade through, and the duration it will take:

```C#
// Create a new screen context with the target scene
var screenContext = new ScreenContext(new SecondScene());

// Create the transition 
var transition = new ColorFadeTransition(Color.Aqua, TimeSpan.FromSeconds(1));

// Navigate to the new screen context
WaveServices.ScreenContextManager.To(screenContext, transition);
```

## Wrap-up

We have learned the three pieces involved in screens navigation: context, transition and context manager. This last one takes the first two in order to execute the navigation. We have pointed out where to find every transition available, and a quick example of how to set all up.