## Goal

Within this recipe you will learn to add buttons to your games. Note that all UI components can be added in the same way.

## Hands-on

### With Wave Visual Editor

From an empty project, open the solution with **Visual Studio** or **Xamarin Studio**

Note: At the time of writing this recipe, Wave Visual Editor does not support adding UI components, so continue reading please.

### With Visual Studio/Xamarin Studio

Edit *CreateScene* method from *MyScene* class with the code that will add a button to the scene:

```C#
Button button = new Button("Hello World")
{
    HorizontalAlignment = HorizontalAlignment.Center,## Wrap-up
    VerticalAlignment = VerticalAlignment.Center,
};

EntityManager.Add(button.Entity);
```

*Button* class and every component in *WaveEngine.Components.UI* namespace have very useful properties to play with, please feel free to tree them out.

## Wrap-up

You have learned how to add an UI component to your scenes.