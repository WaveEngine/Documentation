## Goal

Within this recipe you will learn how to use the device accelerometer in your games 

## Hands-on

_Note_: The accelerometer cannot currently be handled from Wave Visual Editor, so we will concentrate on the IDEs

### With Visual Studio/Xamarin Studio

You must start with this code to make Wave Engine works with the hardware accelerometer:

```C#
WaveServices.Input.StartAccelerometer();
```

Now, you can access the accelerometer readings with this code:

```C#
AccelerometerState state = WaveServices.Input.AccelerometerState;
```

[AccelerometerState](xref:WaveEngine.Common.Input.AccelerometerState)
exposes the following information:

| Property | Description |
|---	|---	|
|IsConnected| **true** if the accelerometer is available, **false** in other case|
|RawAcceleration| Raw acceleration value in G-force.|
|SmoothAcceleration | Smooth acceleration value in G-force.|

## Wrap-up

You can now use the accelerometer to make your game more interactive