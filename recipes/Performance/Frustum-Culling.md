## Goal

Frustum Culling is the technique which does not draw those things which are out of the camera scope. Wave Engine has enabled by default such algorithm, but it is important to highlight some key points in order to assure its correct behavior. You can read more on Frustum Culling technique [here](https://en.wikipedia.org/wiki/Hidden_surface_determination).

## Hands-on

Every [Drawable2D](xref:Wave​Engine.​Framework.​Graphics.Drawable2D) and [Drawable3D](xref:Wave​Engine.​Framework.​Graphics.Drawable3D) component supports Frustum Culling technique through the [CullingEnabled](xref:Wave​Engine.​Framework.​Graphics.Drawable2D.CullingEnabled) and [FrustumCullingEnabled](xref:Wave​Engine.​Framework.​Graphics.Drawable3D.FrustumCullingEnabled) properties, respectively, which are `true` by default. In order to Wave Engine detect whether an object is inside or outside the scope of the camera, such needs to pack a collider. Please refer to [Detect 3d collisions](../Graphics3D/Detect-3D-Collisions.md) and [Detect 2D collisions](../Graphics2D/Detect-2d-collisions.md) which focus on how to work with colliders -this will not be covered here.

[FrustumCulling](https://github.com/WaveEngine/Samples/tree/master/Performance/FrustumCulling) sample graphically shows entities dis/appearing on demand, with a mesh representing the frustum culling area.

### With Wave Visual Editor

Since Wave Visual Editor supports seeing in real time the current scene rendered, it also supplies diagnostic info, with culling counters included. Follow [[Activate diagnostics mode]] to achieve this. The "Culled" label which will be printed on the top left corner of the Viewport will do just that.

### With Visual Studio/Xamarin Studio

This technique can be disabled globally also through [RenderManager](xref:Wave​Engine.​Framework.​Managers.RenderManager)'s [FrustumCullingEnabled](xref:Wave​Engine.​Framework.​Managers.RenderManager.FrustumCullingEnabled) property, setting it as `false`.

Further, this recipe does not involve source code which would require Visual Studio/Xamarin Studio.

## Wrap-up

Wave Engine has built-in support for Frustum Culling technique, which does not draw those meshes which are out of the camera scope. Such is activated by default, but require entities to pack a collider.