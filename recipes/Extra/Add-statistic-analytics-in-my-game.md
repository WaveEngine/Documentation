## Goal

Within this recipe you will learn how to integrate in your games **Localytics** to track users, sessions, devices and geography.

## Hands-on

Log-in into [Localytics Dashboard](http://www.localytics.com/) and register a **Windows Phone 7** app to retrieve an app key for your Wave Engine project.

Just open the project with **Visual Studio** or **Xamarin Studio**.

### With Visual Studio/Xamarin Studio

Add a reference to **WaveEngine.Analytics** package from NuGet.

Add an *AnalyticsManager* object type member in Game.cs file:

```C#
public class Game : WaveEngine.Framework.Game
{
        protected AnalyticsManager analyticsManager;
...
```

Add this code to setup the *AnalyticsManager* in the *Initialize* method, register it on *WaveServices*, and configure its app key:

```C#
this.analyticsManager = new AnalyticsManager(application.Adapter);
WaveServices.RegisterService<AnalyticsManager>(analyticsManager);

this.analyticsManager.SetAnalyticsSystem(new LocalyticsInfo(/*Insert your Localytics API KEY code here*/));
this.analyticsManager.Open();
this.analyticsManager.Upload();
```

Override *OnActivated* and *OnDeactivated* methods in this way to open and close the flow between your app and **Localytics** servers:
```C#
public override void OnActivated()
{
    base.OnActivated();

    if (!this.analyticsManager.IsOpen)
    {
        this.analyticsManager.Open();
        this.analyticsManager.Upload();
    }
}

public override void OnDeactivated()
{
    base.OnDeactivated();

    analyticsManager.Close();
}        
```
To track events for **Localytics** just call to *TagEvent* method from *AnalyticsManager* service now:

```C#
 WaveServices.GetService<AnalyticsManager>().TagEvent("CharacterSelection", "Character", "Spanker");
```

That is all need to track the information you want using the **Localytics** server.

## Wrap-up

You have learned how to use **Localytics** to track events in your games.The tracked events tracked will not be available on **Localytics Dashboard** until some minutes later so be patient.
