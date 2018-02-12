---
title: "ASP.NET Core 的設定"
author: rick-anderson
description: "透過多種方法來使用組態 API 設定 ASP.NET Core 應用程式。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/index
ms.openlocfilehash: 6b9dcfcc2fa380b601eee56095f2e6a6dbe07732
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="configure-an-aspnet-core-app"></a><span data-ttu-id="8c6e0-103">設定 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="8c6e0-103">Configure an ASP.NET Core App</span></span>

<span data-ttu-id="8c6e0-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Mark Michaelis](http://intellitect.com/author/mark-michaelis/)、[Steve Smith](https://ardalis.com/)、[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8c6e0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="8c6e0-105">組態 API 可讓您根據成對的名稱和數值清單來設定 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="8c6e0-106">組態是在執行階段讀取自多個來源。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="8c6e0-107">成對的名稱和數值可以分組成多重層級階層。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-107">Name-value pairs can be grouped into a multi-level hierarchy.</span></span>

<span data-ttu-id="8c6e0-108">下列項目有組態提供者：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-108">There are configuration providers for:</span></span>

* <span data-ttu-id="8c6e0-109">檔案格式 (INI、JSON 和 XML)</span><span class="sxs-lookup"><span data-stu-id="8c6e0-109">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="8c6e0-110">命令列引數</span><span class="sxs-lookup"><span data-stu-id="8c6e0-110">Command-line arguments</span></span>
* <span data-ttu-id="8c6e0-111">環境變數</span><span class="sxs-lookup"><span data-stu-id="8c6e0-111">Environment variables</span></span>
* <span data-ttu-id="8c6e0-112">記憶體內部 .NET 物件</span><span class="sxs-lookup"><span data-stu-id="8c6e0-112">In-memory .NET objects</span></span>
* <span data-ttu-id="8c6e0-113">加密的使用者存放區</span><span class="sxs-lookup"><span data-stu-id="8c6e0-113">An encrypted user store</span></span>
* [<span data-ttu-id="8c6e0-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="8c6e0-114">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="8c6e0-115">自訂提供者 (已安裝或已建立)</span><span class="sxs-lookup"><span data-stu-id="8c6e0-115">Custom providers (installed or created)</span></span>

<span data-ttu-id="8c6e0-116">每個組態值會對應至一個字串索引鍵。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="8c6e0-117">還有內建繫結支援可將設定還原序列化為自訂 [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) 物件 (具有屬性的簡單 .NET 類別)。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="8c6e0-118">選項模式使用選項類別來代表一組相關的設定。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="8c6e0-119">如需使用選項模式的詳細資訊，請參閱[選項](xref:fundamentals/configuration/options)主題。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="8c6e0-120">[檢視或下載範例程式碼](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8c6e0-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="8c6e0-121">JSON 組態</span><span class="sxs-lookup"><span data-stu-id="8c6e0-121">JSON configuration</span></span>

<span data-ttu-id="8c6e0-122">下列主控台應用程式使用 JSON 組態提供者：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="8c6e0-123">此應用程式會讀取並顯示下列組態設定：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="8c6e0-124">組態是由成對的名稱和數值階層式清單所組成，其中節點是以冒號分隔。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="8c6e0-125">若要擷取值，請使用對應項目的索引鍵來存取 `Configuration` 索引子：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="8c6e0-126">若要使用 JSON 格式組態來源中的陣列，請使用陣列索引作為冒號分隔字串的一部分。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="8c6e0-127">下列範例會取得上述 `wizards` 陣列中第一個項目的名稱：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="8c6e0-128">寫入內建[組態](/dotnet/api/microsoft.extensions.configuration)提供者的成對名稱和數值**不會**保存。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-128">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="8c6e0-129">不過，可以建立自訂提供者來儲存值。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-129">However, a custom provider that saves values can be created.</span></span> <span data-ttu-id="8c6e0-130">請參閱[自訂組態提供者](xref:fundamentals/configuration/index#custom-config-providers)。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="8c6e0-131">上述範例使用 Configuration 索引子來讀取值。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="8c6e0-132">若要存取 `Startup` 外部組態，請使用「選項模式」。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="8c6e0-133">如需詳細資訊，請參閱[選項](xref:fundamentals/configuration/options)主題。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>


## <a name="configuration-by-environment"></a><span data-ttu-id="8c6e0-134">取決於環境的組態</span><span class="sxs-lookup"><span data-stu-id="8c6e0-134">Configuration by environment</span></span>

<span data-ttu-id="8c6e0-135">不同的環境 (例如開發、測試和生產) 通常會有不同的組態設定。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-135">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="8c6e0-136">ASP.NET Core 2.x 應用程式中的 `CreateDefaultBuilder` 擴充方法 (或在 ASP.NET Core 1.x 應用程式中直接使用 `AddJsonFile` 和 `AddEnvironmentVariables`) 會新增組態提供者以讀取 JSON 檔案和系統組態來源：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-136">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="8c6e0-137">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="8c6e0-137">*appsettings.json*</span></span>
* <span data-ttu-id="8c6e0-138">*appsettings.\<環境名稱>.json*</span><span class="sxs-lookup"><span data-stu-id="8c6e0-138">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="8c6e0-139">環境變數</span><span class="sxs-lookup"><span data-stu-id="8c6e0-139">Environment variables</span></span>

<span data-ttu-id="8c6e0-140">ASP.NET Core 1.x 應用程式需要呼叫 `AddJsonFile` 與 [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_)。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-140">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="8c6e0-141">如需參數的說明，請參閱 [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions)。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-141">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="8c6e0-142">`reloadOnChange` 只有在 ASP.NET Core 1.1 和更新版本中才支援。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-142">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="8c6e0-143">組態來源是依其指定順序讀取。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-143">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="8c6e0-144">在上述程式碼中，環境變數最後才會讀取。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-144">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="8c6e0-145">在環境中設定的任何組態值會取代在上述兩個提供者中設定的值。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-145">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="8c6e0-146">請考慮使用下列 *appsettings.Staging.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-146">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[Main](index/sample/appsettings.Staging.json)]

<span data-ttu-id="8c6e0-147">當環境設定為 `Staging` 時，下列 `Configure` 方法會讀取 `MyConfig` 的值：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-147">When the environment is set to `Staging`, the following `Configure` method reads the value of `MyConfig`:</span></span>

[!code-csharp[Main](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]


<span data-ttu-id="8c6e0-148">環境通常會設定為 `Development`、`Staging` 或 `Production`。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-148">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="8c6e0-149">如需詳細資訊，請參閱[使用多個環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-149">For more information, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="8c6e0-150">組態考量：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-150">Configuration considerations:</span></span>

* <span data-ttu-id="8c6e0-151">`IOptionsSnapshot` 可在組態資料變更時重新載入資料。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-151">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="8c6e0-152">如需詳細資訊，請參閱 [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot)。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-152">For more information, see [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).,</span></span>
* <span data-ttu-id="8c6e0-153">組態金鑰**不**區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-153">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="8c6e0-154">**永遠不要**將密碼或其他敏感性資料儲存在組態提供者程式碼或純文字組態檔中。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-154">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="8c6e0-155">不要在開發或測試環境中使用生產環境祕密。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-155">Don't use production secrets in development or test environments.</span></span> <span data-ttu-id="8c6e0-156">請在專案外部指定祕密，以防止其意外認可至開放原始碼存放庫。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-156">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span> <span data-ttu-id="8c6e0-157">深入了解如何[使用多個環境](xref:fundamentals/environments)及管理[在開發期間安全儲存應用程式祕密](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-157">Learn more about [working with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets during development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="8c6e0-158">如果無法在系統上的環境變數中使用冒號 (`:`)，請以雙底線 (`__`) 取代冒號 (`:`)。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-158">If a colon (`:`) can't be used in environment variables on a system, replace the colon (`:`) with a double-underscore (`__`).</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="8c6e0-159">記憶體內部提供者和 POCO 類別的繫結</span><span class="sxs-lookup"><span data-stu-id="8c6e0-159">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="8c6e0-160">下列範例示範如何使用記憶體內部提供者，並繫結至一個類別：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-160">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

<span data-ttu-id="8c6e0-161">組態值是以字串傳回，但繫結會啟用物件的建構。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-161">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="8c6e0-162">使用繫結即可擷取 POCO 物件，甚至是整個物件圖形。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-162">Binding allows the retrieval of POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="8c6e0-163">GetValue</span><span class="sxs-lookup"><span data-stu-id="8c6e0-163">GetValue</span></span>

<span data-ttu-id="8c6e0-164">下列範例示範 [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-164">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="8c6e0-165">使用 ConfigurationBinder 的 `GetValue<T>` 方法可指定預設值 (在範例中為 80)。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-165">The ConfigurationBinder's `GetValue<T>` method allows the specification of a default value (80 in the sample).</span></span> <span data-ttu-id="8c6e0-166">`GetValue<T>` 適用於簡單的案例，並不會繫結至整個區段。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-166">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="8c6e0-167">`GetValue<T>` 會從轉換成特定類型的 `GetSection(key).Value` 取得純量值。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-167">`GetValue<T>` obtains scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="8c6e0-168">繫結至物件圖形</span><span class="sxs-lookup"><span data-stu-id="8c6e0-168">Bind to an object graph</span></span>

<span data-ttu-id="8c6e0-169">類別中的每個物件都可以遞迴繫結。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-169">Each object in a class can be recursively bound.</span></span> <span data-ttu-id="8c6e0-170">請考慮下列 `AppSettings` 類別：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-170">Consider the following `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="8c6e0-171">下列範例會繫結至 `AppSettings` 類別：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-171">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="8c6e0-172">**ASP.NET Core 1.1** 和更新版本可以使用 `Get<T>`，這適用於整個區段。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-172">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="8c6e0-173">`Get<T>` 可能比使用 `Bind` 更方便。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-173">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="8c6e0-174">下列程式碼示範如何在上述範例中使用 `Get<T>`：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-174">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="8c6e0-175">使用下列 *appsettings.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-175">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="8c6e0-176">此程式會顯示 `Height 11`。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-176">The program displays `Height 11`.</span></span>

<span data-ttu-id="8c6e0-177">下列程式碼可用於組態的單元測試：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-177">The following code can be used to unit test the configuration:</span></span>

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="8c6e0-178">建立 Entity Framework 自訂提供者</span><span class="sxs-lookup"><span data-stu-id="8c6e0-178">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="8c6e0-179">在本節中，會建立從使用 EF 的資料庫讀取成對的名稱和數值的基本組態提供者。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-179">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="8c6e0-180">定義 `ConfigurationValue` 實體來將組態值儲存在資料庫中：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-180">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="8c6e0-181">新增 `ConfigurationContext` 來儲存及存取所設定的值：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-181">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="8c6e0-182">建立實作 [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource) 的類別：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-182">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="8c6e0-183">透過繼承自 [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider) 來建立自訂組態提供者。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-183">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="8c6e0-184">組態提供者會將空白資料庫初始化：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-184">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="8c6e0-185">執行範例時，會顯示資料庫中醒目提示的值 ("value_from_ef_1" 和 "value_from_ef_2") 。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-185">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="8c6e0-186">`EFConfigSource` 擴充方法可以用來新增組態來源：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-186">An `EFConfigSource` extension method for adding the configuration source can be used:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="8c6e0-187">下列程式碼示範如何使用自訂 `EFConfigProvider`：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-187">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="8c6e0-188">請注意，此範例會在 JSON 提供者後面新增自訂 `EFConfigProvider`，如此一來資料庫中的任何設定就會覆寫 *appsettings.json* 檔案中的設定。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-188">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="8c6e0-189">使用下列 *appsettings.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-189">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="8c6e0-190">下列輸出隨即顯示：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-190">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="8c6e0-191">命令列組態提供者</span><span class="sxs-lookup"><span data-stu-id="8c6e0-191">CommandLine configuration provider</span></span>

<span data-ttu-id="8c6e0-192">[命令列組態提供者](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider)會在執行階段接收組態的命令列引數索引鍵/值組。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-192">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="8c6e0-193">檢視或下載命令列組態範例</span><span class="sxs-lookup"><span data-stu-id="8c6e0-193">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="8c6e0-194">設定及使用命令列組態提供者</span><span class="sxs-lookup"><span data-stu-id="8c6e0-194">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="8c6e0-195">基本組態</span><span class="sxs-lookup"><span data-stu-id="8c6e0-195">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="8c6e0-196">若要啟用命令列組態，請在 [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) 的執行個體上呼叫 `AddCommandLine` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-196">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="8c6e0-197">執行程式碼，隨即顯示下列輸出：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-197">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="8c6e0-198">在命令列上傳遞引數索引鍵/值組會變更 `Profile:MachineName` 和 `App:MainWindow:Left` 的值：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-198">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="8c6e0-199">主控台視窗會顯示：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-199">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="8c6e0-200">若要使用命令列組態來覆寫其他組態提供者所提供的組態，請在 `ConfigurationBuilder` 上最後才呼叫 `AddCommandLine`：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-200">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8c6e0-201">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8c6e0-201">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8c6e0-202">一般 ASP.NET Core 2.x 應用程式會使用靜態簡便方法 `CreateDefaultBuilder` 來建置主應用程式：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-202">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="8c6e0-203">`CreateDefaultBuilder` 會從 *appsettings.json*、*appsettings.{Environment}.json*、[使用者祕密](xref:security/app-secrets) (在 `Development` 環境中)、環境變數和命令列引數載入選擇性組態。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-203">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="8c6e0-204">最後才呼叫命令列組態提供者。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-204">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="8c6e0-205">最後呼叫提供者可讓在執行階段傳遞的命令列引數，覆寫之前呼叫之其他組態提供者所設定的組態。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-205">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="8c6e0-206">若是符合下列條件的 *appsettings* 檔案：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-206">For *appsettings* files where:</span></span>

* <span data-ttu-id="8c6e0-207">`reloadOnChange` 已啟用。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-207">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="8c6e0-208">在命令列引數與 *appsettings* 檔案中包含相同設定。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-208">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="8c6e0-209">包含相符命令列引數的 *appsettings* 檔案在應用程式啟動後變更。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-209">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="8c6e0-210">若上述條件皆成立，即覆寫所有命令列引數。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-210">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="8c6e0-211">ASP.NET Core 2.x 可以使用 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 來代替 `CreateDefaultBuilder`。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-211">ASP.NET Core 2.x app can use [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of `CreateDefaultBuilder`.</span></span> <span data-ttu-id="8c6e0-212">使用 `WebHostBuilder` 時，請以 [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) 手動進行設定。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-212">When using `WebHostBuilder`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="8c6e0-213">如需詳細資訊，請參閱 ASP.NET Core 1.x 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-213">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8c6e0-214">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8c6e0-214">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8c6e0-215">建立 [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) 並呼叫 `AddCommandLine` 方法來使用命令列組態提供者。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-215">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="8c6e0-216">最後呼叫提供者可讓在執行階段傳遞的命令列引數，覆寫之前呼叫之其他組態提供者所設定的組態。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-216">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="8c6e0-217">使用 `UseConfiguration` 方法將組態套用至 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-217">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="8c6e0-218">引數</span><span class="sxs-lookup"><span data-stu-id="8c6e0-218">Arguments</span></span>

<span data-ttu-id="8c6e0-219">在命令列上傳遞的引數必須符合下表所示的兩種格式之一：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-219">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="8c6e0-220">引數格式</span><span class="sxs-lookup"><span data-stu-id="8c6e0-220">Argument format</span></span>                                                     | <span data-ttu-id="8c6e0-221">範例</span><span class="sxs-lookup"><span data-stu-id="8c6e0-221">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="8c6e0-222">單一引數：以等號 (`=`) 分隔的索引鍵/值組</span><span class="sxs-lookup"><span data-stu-id="8c6e0-222">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="8c6e0-223">連續兩個引數：以空格分隔的索引鍵/值組</span><span class="sxs-lookup"><span data-stu-id="8c6e0-223">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="8c6e0-224">**單一引數**</span><span class="sxs-lookup"><span data-stu-id="8c6e0-224">**Single argument**</span></span>

<span data-ttu-id="8c6e0-225">值必須在等號 (`=`) 後面。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-225">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="8c6e0-226">這個值可以是 Null (例如 `mykey=`)。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-226">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="8c6e0-227">索引鍵可能會有前置字元。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-227">The key may have a prefix.</span></span>

| <span data-ttu-id="8c6e0-228">索引鍵前置字元</span><span class="sxs-lookup"><span data-stu-id="8c6e0-228">Key prefix</span></span>               | <span data-ttu-id="8c6e0-229">範例</span><span class="sxs-lookup"><span data-stu-id="8c6e0-229">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="8c6e0-230">沒有前置字元</span><span class="sxs-lookup"><span data-stu-id="8c6e0-230">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="8c6e0-231">單虛線 (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="8c6e0-231">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="8c6e0-232">雙虛線 (`--`)</span><span class="sxs-lookup"><span data-stu-id="8c6e0-232">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="8c6e0-233">正斜線 (`/`)</span><span class="sxs-lookup"><span data-stu-id="8c6e0-233">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="8c6e0-234">&#8224;您必須在[切換對應](#switch-mappings)中提供具有單虛線前置字元 (`-`) 的索引鍵，如下所述。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-234">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="8c6e0-235">範例命令：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-235">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="8c6e0-236">注意：如果提供給組態提供者的[切換對應](#switch-mappings)中沒有 `-key1`，則會擲回 `FormatException`。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-236">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="8c6e0-237">**連續兩個引數**</span><span class="sxs-lookup"><span data-stu-id="8c6e0-237">**Sequence of two arguments**</span></span>

<span data-ttu-id="8c6e0-238">值不可以是 Null，而且必須在以空格分隔的索引鍵後面。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-238">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="8c6e0-239">索引鍵必須有前置字元。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-239">The key must have a prefix.</span></span>

| <span data-ttu-id="8c6e0-240">索引鍵前置字元</span><span class="sxs-lookup"><span data-stu-id="8c6e0-240">Key prefix</span></span>               | <span data-ttu-id="8c6e0-241">範例</span><span class="sxs-lookup"><span data-stu-id="8c6e0-241">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="8c6e0-242">單虛線 (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="8c6e0-242">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="8c6e0-243">雙虛線 (`--`)</span><span class="sxs-lookup"><span data-stu-id="8c6e0-243">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="8c6e0-244">正斜線 (`/`)</span><span class="sxs-lookup"><span data-stu-id="8c6e0-244">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="8c6e0-245">&#8224;您必須在[切換對應](#switch-mappings)中提供具有單虛線前置字元 (`-`) 的索引鍵，如下所述。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-245">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="8c6e0-246">範例命令：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-246">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="8c6e0-247">注意：如果提供給組態提供者的[切換對應](#switch-mappings)中沒有 `-key1`，則會擲回 `FormatException`。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-247">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="8c6e0-248">重複索引鍵</span><span class="sxs-lookup"><span data-stu-id="8c6e0-248">Duplicate keys</span></span>

<span data-ttu-id="8c6e0-249">如果提供了重複索引鍵，則會使用最後一個索引鍵/值組。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-249">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="8c6e0-250">切換對應</span><span class="sxs-lookup"><span data-stu-id="8c6e0-250">Switch mappings</span></span>

<span data-ttu-id="8c6e0-251">使用 `ConfigurationBuilder` 手動建置組態時，可以將參數對應字典到 `AddCommandLine` 方法。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-251">When manually building configuration with `ConfigurationBuilder`, a switch mappings dictionary can be added to the `AddCommandLine` method.</span></span> <span data-ttu-id="8c6e0-252">參數對應允許索引鍵名稱取代邏輯。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-252">Switch mappings allow key name replacement logic.</span></span>

<span data-ttu-id="8c6e0-253">使用切換對應字典時，會檢查字典中是否有任何索引鍵符合命令列引數所提供的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-253">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="8c6e0-254">如果在字典中找到命令列索引鍵，則會傳回字典值 (索引鍵取代) 以設定組態。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-254">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="8c6e0-255">所有前面加上單虛線 (`-`) 的命令列索引鍵都需要切換對應。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-255">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="8c6e0-256">切換對應字典索引鍵規則：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-256">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="8c6e0-257">切換必須以單虛線 (`-`) 或雙虛線 (`--`) 開頭。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-257">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="8c6e0-258">切換對應字典不能包含重複索引鍵。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-258">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="8c6e0-259">在下列範例中，`GetSwitchMappings` 方法可讓命令列引數使用單虛線 (`-`) 索引鍵前置字元，並避免以子索引鍵前置字元開頭。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-259">In the following example, the `GetSwitchMappings` method allows command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="8c6e0-260">若未提供命令列引數，就會由提供給 `AddInMemoryCollection` 的字典來設定組態值。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-260">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="8c6e0-261">使用下列命令來執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-261">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="8c6e0-262">主控台視窗會顯示：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-262">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="8c6e0-263">使用下列命令在組態設定中傳遞：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-263">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="8c6e0-264">主控台視窗會顯示：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-264">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="8c6e0-265">建立參數對應字典之後，其中會包含下表所示的資料：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-265">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="8c6e0-266">Key</span><span class="sxs-lookup"><span data-stu-id="8c6e0-266">Key</span></span>            | <span data-ttu-id="8c6e0-267">值</span><span class="sxs-lookup"><span data-stu-id="8c6e0-267">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="8c6e0-268">若要示範如何使用字典切換索引鍵，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-268">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="8c6e0-269">命令列索引鍵會互換。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-269">The command-line keys are swapped.</span></span> <span data-ttu-id="8c6e0-270">主控台視窗會顯示 `Profile:MachineName` 和 `App:MainWindow:Left` 的組態值：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-270">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a><span data-ttu-id="8c6e0-271">web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="8c6e0-271">The web.config file</span></span>

<span data-ttu-id="8c6e0-272">當您在 IIS 或 IIS Express 中裝載應用程式時，需要 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-272">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="8c6e0-273">*web.config* 中的設定可讓 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)啟動應用程式，並設定其他 IIS 設定和模組。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-273">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="8c6e0-274">如果沒有 *web.config* 檔案，而專案檔包含 `<Project Sdk="Microsoft.NET.Sdk.Web">`，發行專案會在已發行的輸出 (*publish* 資料夾) 中建立 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-274">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="8c6e0-275">如需詳細資訊，請參閱[在使用 IIS 的 Windows 上裝載 ASP.NET Core](xref:host-and-deploy/iis/index#webconfig)。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-275">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig).</span></span>

## <a name="accessing-configuration-during-startup"></a><span data-ttu-id="8c6e0-276">在啟動期間存取組態</span><span class="sxs-lookup"><span data-stu-id="8c6e0-276">Accessing configuration during startup</span></span>

<span data-ttu-id="8c6e0-277">若要在啟動期間存取 `ConfigureServices` 或 `Configure` 內的組態，請參閱[應用程式啟動](xref:fundamentals/startup)主題中的範例。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-277">To access configuration within `ConfigureServices` or `Configure` during startup, see the examples in the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="8c6e0-278">其他備註</span><span class="sxs-lookup"><span data-stu-id="8c6e0-278">Additional notes</span></span>

* <span data-ttu-id="8c6e0-279">相依性插入 (DI) 會在叫用 `ConfigureServices` 之後才設定。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-279">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="8c6e0-280">組態系統無法感知 DI。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-280">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="8c6e0-281">`IConfiguration` 有兩個特製化：</span><span class="sxs-lookup"><span data-stu-id="8c6e0-281">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="8c6e0-282">`IConfigurationRoot` 用於根節點。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-282">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="8c6e0-283">可觸發重新載入。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-283">Can trigger a reload.</span></span>
  * <span data-ttu-id="8c6e0-284">`IConfigurationSection` 代表組態值區段。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-284">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="8c6e0-285">`GetSection` 和 `GetChildren` 方法會傳回 `IConfigurationSection`。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-285">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="8c6e0-286">在重新載入組態或存取各個提供者時使用 [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot)。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-286">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or for access to each provider.</span></span> <span data-ttu-id="8c6e0-287">以上皆為罕見案例。</span><span class="sxs-lookup"><span data-stu-id="8c6e0-287">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8c6e0-288">其他資源</span><span class="sxs-lookup"><span data-stu-id="8c6e0-288">Additional resources</span></span>

* [<span data-ttu-id="8c6e0-289">選項</span><span class="sxs-lookup"><span data-stu-id="8c6e0-289">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="8c6e0-290">使用多個環境</span><span class="sxs-lookup"><span data-stu-id="8c6e0-290">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="8c6e0-291">在開發期間安全儲存應用程式密碼</span><span class="sxs-lookup"><span data-stu-id="8c6e0-291">Safe storage of app secrets during development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="8c6e0-292">在 ASP.NET Core 中裝載</span><span class="sxs-lookup"><span data-stu-id="8c6e0-292">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="8c6e0-293">相依性插入</span><span class="sxs-lookup"><span data-stu-id="8c6e0-293">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="8c6e0-294">Azure Key Vault 組態提供者</span><span class="sxs-lookup"><span data-stu-id="8c6e0-294">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)