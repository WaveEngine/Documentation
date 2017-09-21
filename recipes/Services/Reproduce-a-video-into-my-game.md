## Goal

Within this recipe you will learn how to reproduce a video file and using it as a texture in your games using the built-in VideoPlayer Service.

## Hands-on

### With Visual Studio/Xamarin Studio

WaveEngine comes with a video player service out of the box. This service allows us to reproduce a video and get the texture to be used as we need. 

First we need to create a `VideoInfo` object from a previously added video file from the Visual Editor:

```C#
 var bunnyVideo = WaveServices.VideoPlayer.VideoInfoFromPath(WaveContent.Assets.Video.bear_mp4);
```

Note: The video used in this recipe can be found in the [VideoPlayer example](https://github.com/WaveEngine/Samples/tree/master/Media/VideoPlayer/Content/Assets/Video).

Now we can configure the video player and start to reproduce the video:

```C#
 WaveServices.VideoPlayer.IsLooped = true; 
 WaveServices.VideoPlayer.Play(this.bunnyVideo); 
```

[Here](xref:Wave​Engine.​Framework.​Services.VideoPlayer) you can open the WaveEngine reference and review the available properties and methods of the `VideoPlayer` class.

Take a look of the `VideoTexture` interesting property. We can use this texture as we usually do. For example, if we had an entity in our scene that uses a StandardMaterial created using the Visual Editor named MyMaterial, we can set this texture to the Diffuse property:

```C#
 var myMaterial = this.Assets.LoadModel<MaterialModel>(WaveContent.Assets.Materials.MyMaterial).Material as StandardMaterial; 
 myMaterial.Diffuse = WaveServices.VideoPlayer.VideoTexture; 
```

## Wrap-up

We have seen how to reproduce a video in our game. 
