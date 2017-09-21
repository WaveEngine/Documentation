## Goal

Within this recipe you will learn how to use the microphone WaveEngine service to process and record the audio signal.

## Hands-on

### With Visual Studio/Xamarin Studio

A microphone is a peripheral that could be attached or not, so you **need to check if it's connected** prior to use it:

```c#
if(WaveServices.Microphone.IsConnected)
{
	...
}
```

This service allows to **start recording** the attached microphone:

```c#
try
{
    WaveServices.Microphone.Start();
}
catch (ObjectDisposedException odex)
{
    // disposed
}
catch (InvalidOperationException ioex)
{
    // not recording
}
```

Start method can get a path string parameter to store the microphone data directly to file:

```c#
WaveServices.Microphone.Start(pathToFile);
```

This service allows to **stop recording**:

```c#
try
{
    WaveServices.Microphone.Stop();
}
catch(ObjectDisposedException odex)
{
    // disposed
}
catch(InvalidOperationException ioex)
{
    // not recording
}
```

Microphone service has an event that you can subscribe prior to start recording to process the _raw_ signal. The event will raise every time the buffer is full:

```c#
WaveServices.Microphone.DataAvailable += this.Microphone_DataAvailable;
```
```c#
private void Microphone_DataAvailable(object sender, WaveEngine.Common.Media.MicrophoneDataEventArgs e)
{
	// data process
}
```

## Wrap-up

Within this recipe we have manage the microphone peripheral from device. 