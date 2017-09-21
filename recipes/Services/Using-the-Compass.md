## Goal

Within this recipe you will learn how to use the device compass in your games 

## Hands-on

_Note_: The compass cannot currently be handled from Wave Visual Editor, so we will concentrate on the IDEs

### With Visual Studio/Xamarin Studio

You must start with this code to make Wave Engine works with the hardware compass:

```C#
WaveServices.Input.StartCompass();
```

Now you can access the compass readings with this code:

```C#
CompassStatestate = WaveServices.Input.CompassState;
```

[CompassState](xref:WaveEngine.Common.Input.CompassState)
exposes the following information:

| Property | Description |
|---	|---	|
|IsConnected| **true** if the compass is available, **false** in other case.|
|Accuracy| The accuracy of the measure, in rads per seconds.|
|RawGeographicHeading| The raw geographical heading of the device.|
|RawMagneticHeading|  The raw magnetic heading of the device.|  
|SmoothGeographicHeading|  The heading, in rads per seconds, measured counter clockwise from the Earth’s geographic north.|  
|SmoothMagneticHeading|  The heading, in rads per seconds, measured counterclockwise from the Earth’s magnetic north.|  
|TimeStamp|  A timestamp indicating the time at which the compass reading was taken.|

## Wrap-up

You can now use the compass to make your game more interactive