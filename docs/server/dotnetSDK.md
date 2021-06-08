---
sidebar_label: .Net
title: .Net Server SDK
---

:::info Client vs. Server
These docs are for using our .NET SDK in a multi-user, server side context. For client side and single-user contexts, try our [server .NET SDK](client/dotnetSDK)
:::

This SDK is written in C#, and is [open source and hosted on github](https://github.com/statsig-io/dotnet-sdk).

## The Basics {#the-basics}

Get started in a few quick steps.

1. [Create a free account on statsig.com](#step1)
2. [Install the SDK](#step2)
3. [Initialize and use the SDK](#step3)

<a name="step1"></a>

#### Step 1 - Create a free account on [www.statsig.com](https://console.statsig.com/sign_up) {#step-1---create-a-free-account-on-wwwstatsigcom}

You could skip this for now, but you will need an SDK Key and some Feature Gates or Dynamic Configs to use with the SDK in just a minute.

<a name="step2"></a>

#### Step 2 - Install the SDK {#step-2---install-the-sdk}

The package is hosted on [Nuget](https://www.nuget.org/packages/Statsig/). You can either install it from your Visual Studio's Nuget package manager, or through .NET CLI:

```
dotnet add package Statsig --version 0.1.0
```

<a name="step3"></a>
#### Step 3 - Initialize and use the SDK {#step-3---initialize-the-sdk}

Initialize the SDK using a [Server Secret Key from the statsig console](https://console.statsig.com/api_keys):

```cs
using Statsig;
using Statsig.Server;

private double _subPrice

await Statsig.initialize('<secret>')

// Now you can check gates, get configs, log events for your users.
// e.g. if you are running a promotion that offers all users with a @statsig.com email a discounted price on your monthly subscription service,
// 1. you can first use check_gate to see if they are eligible
var user = new StatsigUser {'email' => 'jkw@statsig.com'};
if (Statsig.check_gate(user, 'has_statsig_email'))
{
  // 2. then get the discounted price from dynamic config
  var priceConfigs = await Statsig.GetConfig(user, "special_item_prices");
  _subPrice = priceConfigs.Get<double>("monthly_sub_price", 0.99);
}

...
// 3. log the conversion event - 'purchase_made' - once they make the purchase
StatsigServer.LogEvent(user, "purchase_made", 1, new Dictionary<string, string>(){ { "price", _subPrice.ToString() } });


// 4. shut down the SDK when your application is closing
StatsigServer.Shutdown();
```


## More Information {#more-information}

For more information, see our [SDK documentation on github](https://github.com/statsig-io/dotnet-sdk).