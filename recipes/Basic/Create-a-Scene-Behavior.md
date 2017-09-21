## Goal

[[Component-based Architecture]] showed us "behaviors" is the way to add logic being executed in each game cycle. But how can a scene it-self handle such? Can a scene have custom logic to be executed on each game loop?

The answer is yes, it can. Wave Engine understands this as "scene behaviors", and it's quite similar to the entity's ones, as you will see in the following paragraphs.

## Hands-on

> [!Note] 
> Scene behaviors cannot currently be handled from Wave Visual Editor, so we will concentrate on the IDEs.

Open the C# Solution created from Wave Visual Editor, and add a new class inheriting [SceneBehavior](xref:WaveEngine.Framework.SceneBehavior) within the Shared Project. `SceneBehavior`'s footprint is, as expected, almost the same as [Behavior](xref:WaveEngine.Framework.Behavior) one. It offers the [Update()](xref:WaveEngine.Framework.Behavior.Update(System.TimeSpan)) method which will be executed on every scene cycle.

```c#
public class MySceneBehavior : SceneBehavior
{
    protected override void Update(System.TimeSpan gameTime)
    {            
    }
}
```

Finally, you must add this behavior to the dessired [Scene](xref:WaveEngine.Framework.Scene) inside its [CreateScene()](xref:WaveEngine.Framework.Scene.CreateScene) method:

```C#
this.AddSceneBehavior(new MySceneBehavior(), SceneBehavior.Order.PostUpdate);
```

Please notice [Scene.AddSceneBehavior()](xref:WaveEngine.Framework.Scene.AddSceneBehavior(WaveEngine.Framework.SceneBehavior,WaveEngine.Framework.SceneBehavior.Order)) requires a second parameter of type [SceneBehavior.Order](xref:WaveEngine.Framework.SceneBehavior.Order), which means when you want the behavior to trigger: `PreUpdate` or `PostUpdate`; before every contained entity is updated or, after this, respectively.

## Wrap-up

This article has explained us scene behaviors are the way to add cycle logic to the scenes, in the same way entities' behavior do the same for those.

We have learned how scene behaviors are built, and actually added to the scenes, indicating whether are triggered before or after the whole scene is updated.