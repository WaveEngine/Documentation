## Goal

A layer is a set of render properties to draw any element. Using layers is an effective way to organize your drawable content. You can establish order between layers and between sprites inside those. Each layer is drawn in sequence (_First In First Draw_).

Wave Engine provides a default list of layers but, you can define as many as you need in your game. The default ones (in drawing order) are:
 1. Opaque
 2. Skybox
 3. Alpha
 4. Additive
 5. GUI
 6. Debug

It makes sense, for instance, to draw a semitransparent sprite within the alpha layer _after_ an opaque one within the same name layer. The may be pixels from the second one which will remain visible in the final render.

You will learn within this guide how to work with this layers architecture.

## Hands-on

### With Visual Studio/Xamarin Studio

As [Load a Sprite (Image)](Load-a-Sprite-(Image).md) shows, drawing a sprite involves telling `SpriteRenderer` component which layer to render on, through its ctor.

One option, the most common one, is referring to an existing layer from [DefaultLayers](xref:WaveEngine.Framework.Graphics.DefaultLayers#fields):
```c#
Type layer = DefaultLayers.Alpha;
```

The other one, is to create a new custom layer. Within the Shared Project add a new class inheriting [Layer](xref:WaveEngine.Framework.Graphics.Layer), customizing its [RenderState](xref:WaveEngine.Framework.Graphics.RenderState) properties:
```c#
public class CustomLayer : Layer
{
	public CustomLayer(RenderManager renderManager) : base(renderManager)
	{
	}
	
	protected override void SetDevice()
	{
		this.renderState.BlendMode = BlendMode.AlphaBlend;
		this.renderState.CullMode = CullMode.None;
		this.renderState.DepthMode = DepthMode.Write;
		this.renderState.DepthBias = DepthBias.Zero;
		this.renderState.MaxAnisotropy = AnisotropyLevel.Aniso1x;
		this.renderState.SamplerMode = AddressMode.LinearClamp;
	}
}
```

Your new layer must registered in `RenderManager` in order to use it on the game. So we need to add some code in `CreateScene()` method from our `Scene`:
```c#
CustomLayer customLayer = new CustomLayer(this.RenderManager);
this.RenderManager.RegisterLayer(customLayer);
```

`RegisterLayer()` will insert `CustomLayer` at the end of the layers list (as the last one to draw), but it is possible to change such order using `RegisterLayerAfter()` and `RegisterLayerBefore()`, for instance "we want our to be drawn just after alpha one":
```c#
this.RenderManager.RegisterLayerAfter(customLayer, DefaultLayers.Alpha);
```

Finally, the layer can obtained to further reference it:
```c#
Type layer = customLayer.GetType(); // or typeof(CustomLayerClass) when not instantiated
```

Whichever option is chosen, now you can add the layer to the `SpriteRenderer`:
```c#	
var entity = new Entity()
	// DrawOrder can be used to change the render priority *inside* the layer
	.AddComponent(new Transform2D() { DrawOrder = 0.5f })
	.AddComponent(new Sprite())
	.AddComponent(new SpriteRenderer(layer));
this.EntityManager.Add(entity);
```

## Wrap-up

We have learned how to create custom layers, insert those within default ones' pipeline and reference one to draw sprites with such.