## Goal

Nowadays a lot of apps includes in-app purchase to extend the functionality or provide exclusive content to their users.

WaveEngine have an out of the box `InAppPurchase` service, which allows us to use the common functionality across all mobile platforms supported by WaveEngine.

## Hands-on

We will assume that you have configured the different portals to enable In-App purchase feature and that you have the API key or tokens and you have set some products.

You must take into account that the `Initialize` method must be called before using any of the service's methods:

```C#
await WaveServices.InAppPurchase.Initialize(inAppPurchaseProperties);
```

The `inAppPurchaseProperties` parameter is a Dictionary<string, string> that contains the necessary initialization properties.

No properties are needed for iOS and Windows Phone, so you can call it with a null value.

In the case of Android, the dictionary must include the license key from Android Developer website with the "Android.PublicKey":

```C#
var inAppPurchaseProperties = new Dictionary<string, string>
{
  { "Android.PublicKey", "LICENSE_KEY" }
};
```

Once the service is initialized, you can list the products to be shown to the user:

```C#
List<string> myProductsIds = new List<string>{ "PRODUCT_ID_1", "PRODUCT_ID_2" };
var products = await WaveServices.InAppPurchase.LoadProductsAsync(myProductsIds);
```

When the user selects a product, you can show the native payment process. If the checkout process has been completed, you can send the receipt to your own server:

```C#
public async Task PurchaseProduct(Product product)
{
    if (await WaveServices.InAppPurchase.PurchaseProductAsync(product.ID))
    {
        var receipt = await WaveServices.InAppPurchase.GetReceiptAsync(product.ID);
        //Send receipt to your own server
    }
}
```

Finally, you can use the simulation mode included in the service, that allows to test the process. The simulation mode should be enabled before the service initialization, using the `SimulationMode` property of the service, and calling the `SetSimulatedProducts` method with a list of simulated products.

```C#
this.SimulationMode = true;
this.SetSimulatedProducts(new List<Product>
{
    new Product { ID = "TEST_1", Name = "Test Produc 1", Description = "Test product 1 description.", Type = Product.ProductType.Consumable, FormattedPrice = "0.99$" },
    new Product { ID = "TEST_2", Name = "Test Produc 2", Description = "Test product 2 description.", Type = Product.ProductType.Consumable, FormattedPrice = "4.99$" },
    new Product { ID = "TEST_3", Name = "Test Produc 3", Description = "Test product 3 description.", Type = Product.ProductType.Consumable, FormattedPrice = "9.99$" }
});
```

## Wrap-up

We have learned how to use the `InAppPurchase` service included in WaveEngine. Allowing us to monetize our application and offering more quality content to our users.
