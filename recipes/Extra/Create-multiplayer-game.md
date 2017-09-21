## Goal

Within this recipe you will learn how to synchronize an entity across other games connected to a host.
This recipe creates a scene with an entity which cares about mouse clicks and taps and shows a simple image when that happens. 
This behavior is synchronized in all others connected applications.

## Hands-on

Create a new WaveEngine project using Wave Visual Editor. You can remove the 3D camera and the default light added to the scene.
Add a png image with the name `logo.png` to the project assets.

### With Visual Studio/Xamarin Studio

Add a reference to **WaveEngine.Networking** package from NuGet.

Register the `NetworkService` in the `Initialize` method of Game.cs file:

```C#
	public override void Initialize(IApplication application)
	{
		base.Initialize(application);

		WaveServices.RegisterService(new NetworkService());

		ScreenContext screenContext = new ScreenContext(new MyScene());    
		WaveServices.ScreenContextManager.To(screenContext);

	}
```

Add the [TapAppearBehavior](https://gist.github.com/danielcaceresm/2809c9d98bbff82144b4) to your project.

Create a new class with the name `SyncMyEntityComponent` and extends from `NetworkSyncComponent`:


```C#
    [DataContract]
    public class SyncMyEntityComponent : NetworkSyncComponent
    {
        private Vector2 lastPosition;
        private float lastOpacity;

        [RequiredComponent]
        protected Transform2D transform;

        public override bool NeedSendSyncData()
        {
            return this.lastPosition != this.transform.Position
                || this.lastOpacity != this.transform.Opacity;
        }

        public override void WriteSyncData(OutgoingMessage writer)
        {
            this.lastPosition = this.transform.Position;
            this.lastOpacity = this.transform.Opacity;

            writer.Write(this.lastPosition.X);
            writer.Write(this.lastPosition.Y);
            writer.Write(this.lastOpacity);
        }

        public override void ReadSyncData(IncomingMessage binaryReader)
        {
            this.transform.Position = new Vector2(binaryReader.ReadSingle(), binaryReader.ReadSingle());
            this.transform.Opacity = binaryReader.ReadSingle();
        }
    }
```

This component handles how the entity is synchronized and need to be added to a local entity.
When its `NeedSendSyncData` method return true, the entity is synchronized using the method `WriteSyncData`.
If a `SyncMyEntityComponent` is owned by an entity created by other instance, the `ReadSyncData` is called whenever is needed to refresh the entity properties.

Now we will implement the scene logic.
Add the following field to the class:

```C#
        private const string ApplicationId = "MyGame";
        private const int Port = 1492;
        private const string SceneIdentifier = "MyScene";

        private NetworkManager networkManager;
        private NetworkService networkService;

        private Entity myEntity;
```

In the scene constructor, assign the `NetworkService` field:

```C#
        public MyScene()
        {
            this.networkService = WaveServices.GetService<NetworkService>();
        }
```

When the scene is created, create the entity and add it to the EntityManager:

```C#
        protected override void CreateScene()
        {
            this.Load(WaveContent.Scenes.MyScene);

            this.EntityManager.Find("defaultCamera2D").FindComponent<Camera2D>().CenterScreen();

            this.myEntity = new Entity("Entity-Sync-" + networkService.ClientIdentifier)
                .AddComponent(new Transform2D() { Origin = new Vector2(0.5f) })
                .AddComponent(new Sprite(WaveContent.Assets.logo_png))
                .AddComponent(new SpriteRenderer())
                .AddComponent(new SyncMyEntityComponent())
                .AddComponent(new NetworkBehavior())
                .AddComponent(new TapAppearBehavior());

            this.EntityManager.Add(this.myEntity);
        }
}     
```

Finally in the `Start` method we need to implement the logic to connect to the server:

```C#        
        protected override async void Start()
        {
            base.Start();

            await this.InitializeNetworkConnection();
            this.InitializeNetworkManager();
        }
```

The `InitializeNetworkConnection` method, tries to discover an existing host in the network, if none of them respond in 1 second, it is initialized as host (and the others instances will discover it):

```C#
        private async Task InitializeNetworkConnection()
        {
            var discoveredHost = await this.WaitForDiscoverHostAsync(TimeSpan.FromSeconds(1));

            if (discoveredHost == null)
            {
                discoveredHost = InitializeHost();
            }

            await this.networkService.ConnectAsync(discoveredHost);
        }

        private Host InitializeHost()
        {
            this.networkService.InitializeHost(ApplicationId, Port);
            var host = new Host() { Address = "127.0.0.1", Port = Port };
            return host;
        }

        private async Task<Host> WaitForDiscoverHostAsync(TimeSpan timeOut)
        {
            Host discoveredHost = null;
            HostDiscovered hostDisceveredHandler = (sender, host) =>
            {
                discoveredHost = host;
            };

            this.networkService.HostDiscovered += hostDisceveredHandler;
            this.networkService.DiscoveryHosts(ApplicationId, Port);
            await Task.Delay(timeOut);
            this.networkService.HostDiscovered -= hostDisceveredHandler;

            return discoveredHost;
        }
```

When the connection is ready, the `InitializeNetworkManager` method register the scene and the entity:

```C#
        private void InitializeNetworkManager()
        {
            this.networkManager = this.networkService.RegisterScene(this, SceneIdentifier);
            this.networkManager.AddEntity(this.myEntity);
        }
```

Now the entity position and opacity will be synchronized between all the connected applications.

![Networking recipe](images\CreateMultiplayerGame\Networking.gif)

## Wrap-up

You have learned how to create an entity that will be synchronized in the other games connected to a host.
