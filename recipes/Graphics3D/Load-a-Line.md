## Goal

You will be guided through the steps needed to add a 3D line to an scene. Such primitives are basic meshes -line , rectangle, polygon, arc. This provides an easy way of fast creating lines in your project.

## Hands-on

### With Wave Visual Editor

At Entities Hierarchy panel, right at the top right corner, you will find a "+" icon. Doing click on it, a context menu will display all the available predefined entities, whose contain Lines 3D.

![](images/LoadALine/polygon_Capture_00.PNG)

Just click on one of those, and the primitive will appear at the Viewport.

![](images/LoadALine/polygon_Capture_01.PNG)

Each primitive has a different Line Mesh but to render them you need a Mesh Line Render that its comom for all of them.
Each line mesh has its own properties

![](images/LoadALine/polygon_Capture_02.PNG)

### With Visual Studio (for Windows or Mac)

All lines are based on [LineMeshBase](xref:WaveEngine.Components.Primitives.LineMeshBase) and needs a [LineMeshRenderer3D](xref:WaveEngine.Components.Graphics3D.LineMeshRenderer3D) to be rendered.

Within the dessired `Scene`, inside `CreateScene()` method, add the following lines:

```c#
 var polygon = new Entity("polygon")
				.AddComponent(new Transform3D())                       
				.AddComponent(new LineMeshRender3D())
				.AddComponent(new LinePolygonMesh()) {
				   Vertices = 6,
				   Thickness = 0.1f,	
				};
			
 this.EntityManager.Add(polygon);
```

![](images/LoadALine/polygon_Capture_03.PNG)

## Wrap-up

You have learned what is a basic line, and how to add those both visually and through source code.