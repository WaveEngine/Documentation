## Goal

After the *CreateScene* method of every scene Wave Engine performs all needed operations to make the entities contained in *EntityManager* appear on the scene with the correct properties values.

But, what happen if we add a component outside the *CreateScene* method? Those properties are not calculated, so the component will not look as we want. 

To solve that, Wave Engine has a service to force the update of those controls:
```C#
            WaveServices.Layout.PerformLayout();
```

## Hands-on

### With Wave Visual Editor

Create an empty project and open it with **Visual Studio** or **Xamarin Studio** from *File > Open C# solution*

### With Visual Studio/Xamarin Studio

Open *MyScene.cs* file and add a button to the scene with this code:

```C#
Button button = new Button("Hello World")
{
    HorizontalAlignment = HorizontalAlignment.Center,
    VerticalAlignment = VerticalAlignment.Center,
};

EntityManager.Add(button.Entity);
```

This code will add a button in the center of the screen. Remember to add the following two lines to the using statements:
```C#
using WaveEngine.Components.UI;
using WaveEngine.Framework.UI;
```

If you are adding the code inside the *CreateScene* method all will work right.
Subscribe to the button *Click* event by adding the code:

```C#
button.Click += button_Click;
```

On the event handler method we will add a *Textblock* "outside" of the *CreateScene* method:

```C#
void button_Click(object sender, EventArgs e)
{
    TextBlock txtbloc = new TextBlock()
    {
        Text = "How are you?",
        HorizontalAlignment = HorizontalAlignment.Center,
    };

    EntityManager.Add(txtbloc);    
}
```

When you run the code and click on the button you will see that the *Textblock* appears in the upper left corner. That is because the properties are not calculated still because the Textblock component were not added to the EntityManager in the *CreateScene* method.

To solve this, you must add the code line that makes all layout recalculate the properties of every components on the scene by this way

```C#
TextBlock txtbloc = new TextBlock()
{
    Text = "How are you?",
    HorizontalAlignment = HorizontalAlignment.Center,
};

EntityManager.Add(txtbloc);
WaveServices.Layout.PerformLayout();
```

When you run the code, you will see that the *Textblock* appears in the upper center of the screen.

## Updating individual controls

*PerformLayout* updates all the controls in all the scenes (unless you specify a scene to update). But what if you changed just a single control, but you don't want to update all of them? Say you have changed the orientation of a *StackPanel*. Then you have to call the *Arrange* method of the corresponding *Control*, like so:
```C#
StackPanel stackPanel = new StackPanel();
stackPanel.Orientation = Orientation.Vertical;
...
stackPanel.Orientation = Orientation.Horizontal;
stackPanel.Entity.FindComponent<StackPanelControl>().Arrange(
    stackPanel.Entity.FindComponent<Transform2D>().Rectangle);
```
This will update all the children of the stack panel.

## Wrap-up

You have learned how to add controls outside the *CreateScene* method.