---
title: "ASP.NET 核心模組的組態參考"
author: guardrex
description: "如何設定適用於主控 ASP.NET Core 應用程式的 ASP.NET 核心模組。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: c1a05c3e40e6aab0f2e4a97c0b3bb9eca8a08a41
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="9d725-103">ASP.NET 核心模組的組態參考</span><span class="sxs-lookup"><span data-stu-id="9d725-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="9d725-104">由[Luke Latham](https://github.com/guardrex)， [Rick Anderson](https://twitter.com/RickAndMSFT)，和[Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="9d725-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="9d725-105">本文件提供有關如何設定適用於主控 ASP.NET Core 應用程式的 ASP.NET 核心模組的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="9d725-105">This document provides details on how to configure the ASP.NET Core Module for hosting ASP.NET Core applications.</span></span> <span data-ttu-id="9d725-106">如需 ASP.NET Core 模組和安裝指示的簡介，請參閱[ASP.NET Core 模組概觀](xref:fundamentals/servers/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="9d725-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

## <a name="configuration-via-webconfig"></a><span data-ttu-id="9d725-107">透過 web.config 設定</span><span class="sxs-lookup"><span data-stu-id="9d725-107">Configuration via web.config</span></span>

<span data-ttu-id="9d725-108">ASP.NET 核心模組已透過站台或應用程式設定*web.config*檔案，並有它自己`aspNetCore`組態區段內`system.webServer`。</span><span class="sxs-lookup"><span data-stu-id="9d725-108">The ASP.NET Core Module is configured via a site or application *web.config* file and has its own `aspNetCore` configuration section within `system.webServer`.</span></span> <span data-ttu-id="9d725-109">以下是範例*web.config*檔`Microsoft.NET.Sdk.Web`SDK 會提供當專案發行時[framework 相依的部署](https://docs.microsoft.com/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)使用預留位置`processPath`和`arguments`:</span><span class="sxs-lookup"><span data-stu-id="9d725-109">Here's an example *web.config* file that the `Microsoft.NET.Sdk.Web` SDK will provide when the project is published for a [framework-dependent deployment](https://docs.microsoft.com/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) with placeholders for the `processPath` and `arguments`:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="%LAUNCHER_PATH%" 
        arguments="%LAUNCHER_ARGS%" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="9d725-110">*Web.config*下列範例是針對[獨立的部署](https://docs.microsoft.com/dotnet/articles/core/deploying/#self-contained-deployments-scd)至[Azure App Service](https://azure.microsoft.com/services/app-service/)。</span><span class="sxs-lookup"><span data-stu-id="9d725-110">The *web.config* example below is for a [self-contained deployment](https://docs.microsoft.com/dotnet/articles/core/deploying/#self-contained-deployments-scd) to the [Azure App Service](https://azure.microsoft.com/services/app-service/).</span></span> <span data-ttu-id="9d725-111">如需詳細資訊，請參閱[與 IIS 的 Windows 上的主機](xref:host-and-deploy/iis/index)。</span><span class="sxs-lookup"><span data-stu-id="9d725-111">For more information, see [Host on Windows with IIS](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="9d725-112">請參閱[子應用程式的組態](xref:host-and-deploy/iis/index#configuration-of-sub-applications)的組態相關的重要注意事項*web.config*子應用程式中的檔案。</span><span class="sxs-lookup"><span data-stu-id="9d725-112">See [Configuration of sub-applications](xref:host-and-deploy/iis/index#configuration-of-sub-applications) for an important note pertaining to the configuration of *web.config* files in sub-applications.</span></span>

```xml
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
        stdoutLogEnabled="false" 
        stdoutLogFile="\\?\%home%\LogFiles\stdout" />
  </system.webServer>
</configuration>
```

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="9d725-113">AspNetCore 元素的屬性</span><span class="sxs-lookup"><span data-stu-id="9d725-113">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="9d725-114">屬性</span><span class="sxs-lookup"><span data-stu-id="9d725-114">Attribute</span></span> | <span data-ttu-id="9d725-115">描述</span><span class="sxs-lookup"><span data-stu-id="9d725-115">Description</span></span> |
| --- | --- |
| <span data-ttu-id="9d725-116">processPath</span><span class="sxs-lookup"><span data-stu-id="9d725-116">processPath</span></span> | <p><span data-ttu-id="9d725-117">必要的字串屬性。</span><span class="sxs-lookup"><span data-stu-id="9d725-117">Required string attribute.</span></span></p><p><span data-ttu-id="9d725-118">將會啟動接聽 HTTP 要求的處理序的可執行檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="9d725-118">Path to the executable that will launch a process listening for HTTP requests.</span></span> <span data-ttu-id="9d725-119">支援相對路徑。</span><span class="sxs-lookup"><span data-stu-id="9d725-119">Relative paths are supported.</span></span> <span data-ttu-id="9d725-120">如果路徑是以開頭 '。 '，路徑會被視為相對於網站根目錄。</span><span class="sxs-lookup"><span data-stu-id="9d725-120">If the path begins with '.', the path is considered to be relative to the site root.</span></span></p><p><span data-ttu-id="9d725-121">沒有預設值。</span><span class="sxs-lookup"><span data-stu-id="9d725-121">There's no default value.</span></span></p> |
| <span data-ttu-id="9d725-122">引數</span><span class="sxs-lookup"><span data-stu-id="9d725-122">arguments</span></span> | <p><span data-ttu-id="9d725-123">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="9d725-123">Optional string attribute.</span></span></p><p><span data-ttu-id="9d725-124">可執行檔中指定的引數**processPath**。</span><span class="sxs-lookup"><span data-stu-id="9d725-124">Arguments to the executable specified in **processPath**.</span></span></p><p><span data-ttu-id="9d725-125">預設值為空字串。</span><span class="sxs-lookup"><span data-stu-id="9d725-125">The default value is an empty string.</span></span></p> |
| <span data-ttu-id="9d725-126">startupTimeLimit</span><span class="sxs-lookup"><span data-stu-id="9d725-126">startupTimeLimit</span></span> | <p><span data-ttu-id="9d725-127">選擇性整數屬性。</span><span class="sxs-lookup"><span data-stu-id="9d725-127">Optional integer attribute.</span></span></p><p><span data-ttu-id="9d725-128">此模組，可開始接聽連接埠之處理序的等候秒持續時間。</span><span class="sxs-lookup"><span data-stu-id="9d725-128">Duration in seconds that the module will wait for the executable to start a process listening on the port.</span></span> <span data-ttu-id="9d725-129">如果超過此時間限制，此模組會終止處理序。</span><span class="sxs-lookup"><span data-stu-id="9d725-129">If this time limit is exceeded, the module will kill the process.</span></span> <span data-ttu-id="9d725-130">此模組會嘗試啟動處理序，當它收到新的要求，並將繼續嘗試重新啟動的程序，在後續的內送要求，除非應用程式無法啟動**rapidFailsPerMinute**數目最後一個循環分鐘的時間。</span><span class="sxs-lookup"><span data-stu-id="9d725-130">The module will attempt to launch the process again when it receives a new request and will continue to attempt to restart the process on subsequent incoming requests unless the application fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="9d725-131">預設值為 120。</span><span class="sxs-lookup"><span data-stu-id="9d725-131">The default value is 120.</span></span></p> |
| <span data-ttu-id="9d725-132">shutdownTimeLimit</span><span class="sxs-lookup"><span data-stu-id="9d725-132">shutdownTimeLimit</span></span> | <p><span data-ttu-id="9d725-133">選擇性整數屬性。</span><span class="sxs-lookup"><span data-stu-id="9d725-133">Optional integer attribute.</span></span></p><p><span data-ttu-id="9d725-134">持續時間 （秒） 模組將會等候到正常關機狀態的可執行檔時*app_offline.htm*偵測到的檔案。</span><span class="sxs-lookup"><span data-stu-id="9d725-134">Duration in seconds for which the module will wait for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p><p><span data-ttu-id="9d725-135">預設值為 10。</span><span class="sxs-lookup"><span data-stu-id="9d725-135">The default value is 10.</span></span></p> |
| <span data-ttu-id="9d725-136">rapidFailsPerMinute</span><span class="sxs-lookup"><span data-stu-id="9d725-136">rapidFailsPerMinute</span></span> | <p><span data-ttu-id="9d725-137">選擇性整數屬性。</span><span class="sxs-lookup"><span data-stu-id="9d725-137">Optional integer attribute.</span></span></p><p><span data-ttu-id="9d725-138">指定處理程序中指定的次數**processPath**允許每分鐘的損毀。</span><span class="sxs-lookup"><span data-stu-id="9d725-138">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="9d725-139">如果超過此限制，此模組會停止啟動該分鐘的剩餘的處理程序。</span><span class="sxs-lookup"><span data-stu-id="9d725-139">If this limit is exceeded, the module will stop launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="9d725-140">預設值為 10。</span><span class="sxs-lookup"><span data-stu-id="9d725-140">The default value is 10.</span></span></p> |
| <span data-ttu-id="9d725-141">requestTimeout</span><span class="sxs-lookup"><span data-stu-id="9d725-141">requestTimeout</span></span> | <p><span data-ttu-id="9d725-142">選擇性 timespan 屬性。</span><span class="sxs-lookup"><span data-stu-id="9d725-142">Optional timespan attribute.</span></span></p><p><span data-ttu-id="9d725-143">指定 ASP.NET 核心模組將等候來自接聽 %aspnetcore_port%之處理序的回應持續期間。</span><span class="sxs-lookup"><span data-stu-id="9d725-143">Specifies the duration for which the ASP.NET Core Module will wait for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="9d725-144">預設值為 "00:02:00"。</span><span class="sxs-lookup"><span data-stu-id="9d725-144">The default value is "00:02:00".</span></span></p><p><span data-ttu-id="9d725-145">`requestTimeout`必須指定以分鐘為單位，否則它會預設為 2 分鐘。</span><span class="sxs-lookup"><span data-stu-id="9d725-145">The `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> |
| <span data-ttu-id="9d725-146">stdoutLogEnabled</span><span class="sxs-lookup"><span data-stu-id="9d725-146">stdoutLogEnabled</span></span> | <p><span data-ttu-id="9d725-147">選擇性的 Boolean 屬性。</span><span class="sxs-lookup"><span data-stu-id="9d725-147">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="9d725-148">如果為 true， **stdout**和**stderr**中指定的處理序**processPath**會重新導向至指定的檔案**stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="9d725-148">If true, **stdout** and **stderr** for the process specified in **processPath** will be redirected to the file specified in **stdoutLogFile**.</span></span></p><p><span data-ttu-id="9d725-149">預設值為 false。</span><span class="sxs-lookup"><span data-stu-id="9d725-149">The default value is false.</span></span></p> |
| <span data-ttu-id="9d725-150">stdoutLogFile</span><span class="sxs-lookup"><span data-stu-id="9d725-150">stdoutLogFile</span></span> | <p><span data-ttu-id="9d725-151">選擇性字串屬性。</span><span class="sxs-lookup"><span data-stu-id="9d725-151">Optional string attribute.</span></span></p><p><span data-ttu-id="9d725-152">指定的相對或絕對檔案路徑的**stdout**和**stderr**中指定的處理序從**processPath**將記錄。</span><span class="sxs-lookup"><span data-stu-id="9d725-152">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** will be logged.</span></span> <span data-ttu-id="9d725-153">相對路徑會相對於網站根目錄。</span><span class="sxs-lookup"><span data-stu-id="9d725-153">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="9d725-154">從任何路徑 '。 ' 將會相對於網站根目錄和所有其他路徑將會被視為絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="9d725-154">Any path starting with '.' will be relative to the site root and all other paths will be treated as absolute paths.</span></span> <span data-ttu-id="9d725-155">提供的路徑中任何資料夾必須存在於要建立的記錄檔之模組的順序。</span><span class="sxs-lookup"><span data-stu-id="9d725-155">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="9d725-156">處理序識別碼，時間戳記 (*yyyyMdhms*)，和副檔名 (*.log*) 加上底線分隔符號會加入至最後一個區段**stdoutLogFile**提供。</span><span class="sxs-lookup"><span data-stu-id="9d725-156">The process ID, timestamp (*yyyyMdhms*), and file extension (*.log*) with underscore delimiters are added to the last segment of the **stdoutLogFile** provided.</span></span></p><p><span data-ttu-id="9d725-157">預設值是 `aspnetcore-stdout`。</span><span class="sxs-lookup"><span data-stu-id="9d725-157">The default value is `aspnetcore-stdout`.</span></span></p> |
| <span data-ttu-id="9d725-158">forwardWindowsAuthToken</span><span class="sxs-lookup"><span data-stu-id="9d725-158">forwardWindowsAuthToken</span></span> | <span data-ttu-id="9d725-159">true 或 false。</span><span class="sxs-lookup"><span data-stu-id="9d725-159">true or false.</span></span></p><p><span data-ttu-id="9d725-160">如果為 true，權杖會轉送子處理序做為每個要求的標頭 ' MS-ASPNETCORE-WINAUTHTOKEN' 接聽 %aspnetcore_port%。</span><span class="sxs-lookup"><span data-stu-id="9d725-160">If true, the token will be forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="9d725-161">它負責這個語彙基元，每個要求上呼叫 CloseHandle 該程序。</span><span class="sxs-lookup"><span data-stu-id="9d725-161">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p><p><span data-ttu-id="9d725-162">預設值為 true。</span><span class="sxs-lookup"><span data-stu-id="9d725-162">The default value is true.</span></span></p> |
| <span data-ttu-id="9d725-163">disableStartUpErrorPage</span><span class="sxs-lookup"><span data-stu-id="9d725-163">disableStartUpErrorPage</span></span> | <span data-ttu-id="9d725-164">true 或 false。</span><span class="sxs-lookup"><span data-stu-id="9d725-164">true or false.</span></span></p><p><span data-ttu-id="9d725-165">如果為 true， **502.5-處理序失敗**頁面將會隱藏，而且 502 狀態字碼頁設定在您*web.config*更高的優先順序。</span><span class="sxs-lookup"><span data-stu-id="9d725-165">If true, the **502.5 - Process Failure** page will be suppressed, and the 502 status code page configured in your *web.config* will take precedence.</span></span></p><p><span data-ttu-id="9d725-166">預設值為 false。</span><span class="sxs-lookup"><span data-stu-id="9d725-166">The default value is false.</span></span></p> |

### <a name="setting-environment-variables"></a><span data-ttu-id="9d725-167">設定環境變數</span><span class="sxs-lookup"><span data-stu-id="9d725-167">Setting environment variables</span></span>

<span data-ttu-id="9d725-168">ASP.NET 核心模組可以讓您指定環境變數中指定的處理序`processPath`中一或多個指定這些屬性`environmentVariable`子項目的`environmentVariables`集合項目底下`aspNetCore`項目。</span><span class="sxs-lookup"><span data-stu-id="9d725-168">The ASP.NET Core Module allows you specify environment variables for the process specified in the `processPath` attribute by specifying them in one or more `environmentVariable` child elements of an `environmentVariables` collection element under the `aspNetCore` element.</span></span> <span data-ttu-id="9d725-169">此區段中設定的環境變數優先於系統處理序的環境變數。</span><span class="sxs-lookup"><span data-stu-id="9d725-169">Environment variables set in this section take precedence over system environment variables for the process.</span></span>

<span data-ttu-id="9d725-170">下列範例會設定兩個環境變數。</span><span class="sxs-lookup"><span data-stu-id="9d725-170">The example below sets two environment variables.</span></span> <span data-ttu-id="9d725-171">`ASPNETCORE_ENVIRONMENT`將應用程式的環境設定成`Development`。</span><span class="sxs-lookup"><span data-stu-id="9d725-171">`ASPNETCORE_ENVIRONMENT` will configure the application's environment to `Development`.</span></span> <span data-ttu-id="9d725-172">開發人員可能會暫時將此值設定*web.config*檔案，以便強制[開發人員例外狀況頁面](xref:fundamentals/error-handling)載入偵錯應用程式例外狀況時。</span><span class="sxs-lookup"><span data-stu-id="9d725-172">A developer may temporarily set this value in the *web.config* file in order to force the [developer exception page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="9d725-173">`CONFIG_DIR`是使用者定義的環境變數的範例，開發人員已撰寫的程式碼會讀取啟動，以形成才能載入的應用程式組態檔的路徑上的值。</span><span class="sxs-lookup"><span data-stu-id="9d725-173">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that will read the value on startup to form a path in order to load the app's configuration file.</span></span>

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

## <a name="appofflinehtm"></a><span data-ttu-id="9d725-174">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="9d725-174">app_offline.htm</span></span>

<span data-ttu-id="9d725-175">如果您將檔案名稱*app_offline.htm*根目錄中的 web 應用程式目錄，ASP.NET 核心模組將會嘗試依正常程序關閉應用程式，並停止處理內送要求。</span><span class="sxs-lookup"><span data-stu-id="9d725-175">If you place a file with the name *app_offline.htm* at the root of a web application directory, the ASP.NET Core Module will attempt to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="9d725-176">如果應用程式仍在執行`shutdownTimeLimit`數秒的 ASP.NET 核心模組會終止執行的處理序。</span><span class="sxs-lookup"><span data-stu-id="9d725-176">If the app is still running after `shutdownTimeLimit` number of seconds, the ASP.NET Core Module will kill the running process.</span></span>

<span data-ttu-id="9d725-177">雖然*app_offline.htm*檔案是否存在，ASP.NET 核心模組會回應要求傳送回內容*app_offline.htm*檔案。</span><span class="sxs-lookup"><span data-stu-id="9d725-177">While the *app_offline.htm* file is present, the ASP.NET Core Module will respond to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="9d725-178">一次*app_offline.htm*會移除檔案，在下一個要求載入應用程式，然後回應要求。</span><span class="sxs-lookup"><span data-stu-id="9d725-178">Once the *app_offline.htm* file is removed, the next request loads the application, which then responds to requests.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="9d725-179">啟動錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="9d725-179">Start-up error page</span></span>

<span data-ttu-id="9d725-180">如果 ASP.NET 核心模組無法啟動後端程序或後端程序會啟動但無法在設定的連接埠上接聽，您會看到 HTTP 502.5 狀態字碼頁。</span><span class="sxs-lookup"><span data-stu-id="9d725-180">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, you will see an HTTP 502.5 status code page.</span></span> <span data-ttu-id="9d725-181">若要隱藏這個頁面，並還原至預設 IIS 502 狀態字碼頁，請使用`disableStartUpErrorPage`屬性。</span><span class="sxs-lookup"><span data-stu-id="9d725-181">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="9d725-182">如需有關如何設定自訂錯誤訊息的詳細資訊，請參閱[HTTP 錯誤`<httpErrors>` ](https://docs.microsoft.com/iis/configuration/system.webServer/httpErrors/)。</span><span class="sxs-lookup"><span data-stu-id="9d725-182">For more information on configuring custom error messages, see [HTTP Errors `<httpErrors>`](https://docs.microsoft.com/iis/configuration/system.webServer/httpErrors/).</span></span>

![502 狀態 頁面](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="9d725-184">記錄檔的建立和重新導向</span><span class="sxs-lookup"><span data-stu-id="9d725-184">Log creation and redirection</span></span>

<span data-ttu-id="9d725-185">ASP.NET 核心模組重新導向`stdout`和`stderr`記錄檔磁碟，如果您設定`stdoutLogEnabled`和`stdoutLogFile`屬性`aspNetCore`項目。</span><span class="sxs-lookup"><span data-stu-id="9d725-185">The ASP.NET Core Module redirects `stdout` and `stderr` logs to disk if you set the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element.</span></span> <span data-ttu-id="9d725-186">中的任何資料夾`stdoutLogFile`路徑必須存在於要建立的記錄檔之模組的順序。</span><span class="sxs-lookup"><span data-stu-id="9d725-186">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="9d725-187">建立記錄檔時，會自動加入時間戳記與檔案的副檔名。</span><span class="sxs-lookup"><span data-stu-id="9d725-187">A timestamp and file extension will be added automatically when the log file is created.</span></span> <span data-ttu-id="9d725-188">記錄檔不會進行旋轉，除非處理序回收/重新啟動，就會發生。</span><span class="sxs-lookup"><span data-stu-id="9d725-188">Logs are not rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="9d725-189">它是主機服務提供者來限制記錄檔使用的磁碟空間的責任。</span><span class="sxs-lookup"><span data-stu-id="9d725-189">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span> <span data-ttu-id="9d725-190">使用`stdout`疑難排解應用程式啟動問題而非一般應用程式記錄用途只建議記錄檔。</span><span class="sxs-lookup"><span data-stu-id="9d725-190">Using the `stdout` log is only recommended for troubleshooting application startup issues and not for general application logging purposes.</span></span>

<span data-ttu-id="9d725-191">記錄檔名稱由附加的處理序識別碼 (PID)，時間戳記 (*yyyyMdhms*)，和副檔名 (*.log*) 的最後一個區段以`stdoutLogFile`路徑 (通常*stdout*) 底線字元分隔。</span><span class="sxs-lookup"><span data-stu-id="9d725-191">The log file name is composed by appending the process ID (PID), timestamp (*yyyyMdhms*), and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="9d725-192">例如如果`stdoutLogFile`路徑的結尾*stdout*，記錄檔的應用程式，但在 8/10/2017年端建立 12:05:02 10652 PID 有檔案名稱*stdout_10652_20178101252.log*。</span><span class="sxs-lookup"><span data-stu-id="9d725-192">For example if the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 10652 created on 8/10/2017 at 12:05:02 has the file name *stdout_10652_20178101252.log*.</span></span>

<span data-ttu-id="9d725-193">以下是範例`aspNetCore`設定項目`stdout`記錄。</span><span class="sxs-lookup"><span data-stu-id="9d725-193">Here's a sample `aspNetCore` element that configures `stdout` logging.</span></span> <span data-ttu-id="9d725-194">`stdoutLogFile`範例所示的路徑是否適用於 Azure 應用程式服務。</span><span class="sxs-lookup"><span data-stu-id="9d725-194">The `stdoutLogFile` path shown in the example is appropriate for the Azure App Service.</span></span> <span data-ttu-id="9d725-195">本機路徑或網路共用路徑是可接受的本機記錄。</span><span class="sxs-lookup"><span data-stu-id="9d725-195">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="9d725-196">確認應用程式集區使用者識別必須提供路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="9d725-196">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```
<span data-ttu-id="9d725-197">請參閱[透過 web.config 組態](#configuration-via-webconfig)的範例，`aspNetCore`中的項目*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="9d725-197">See [Configuration via web.config](#configuration-via-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="9d725-198">IIS 與 ASP.NET Core 模組共用設定</span><span class="sxs-lookup"><span data-stu-id="9d725-198">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="9d725-199">ASP.NET 核心模組安裝程式會執行與的權限**系統**帳戶。</span><span class="sxs-lookup"><span data-stu-id="9d725-199">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="9d725-200">因為本機系統帳戶不會有修改 IIS 共用設定所使用的共用路徑的權限，安裝程式會叫用拒絕存取錯誤時嘗試設定中的模組設定*applicationHost.config*共用上。</span><span class="sxs-lookup"><span data-stu-id="9d725-200">Because the local system account doesn't have modify permission for the share path which is used by the IIS Shared Configuration, the installer will hit an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span>

<span data-ttu-id="9d725-201">不支援的因應措施是停用 IIS 共用設定，執行安裝程式、 匯出更新*applicationHost.config*檔案共用，並重新啟用 IIS 共用設定。</span><span class="sxs-lookup"><span data-stu-id="9d725-201">The unsupported workaround is to disable the IIS Shared Configuration, run the installer, export the updated *applicationHost.config* file to the share, and re-enable the IIS Shared Configuration.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="9d725-202">模組、 結構描述和組態檔的位置</span><span class="sxs-lookup"><span data-stu-id="9d725-202">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="9d725-203">Module</span><span class="sxs-lookup"><span data-stu-id="9d725-203">Module</span></span>

<span data-ttu-id="9d725-204">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="9d725-204">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="9d725-205">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="9d725-205">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="9d725-206">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="9d725-206">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="9d725-207">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="9d725-207">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="9d725-208">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="9d725-208">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="9d725-209">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="9d725-209">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="9d725-210">結構描述</span><span class="sxs-lookup"><span data-stu-id="9d725-210">Schema</span></span>

<span data-ttu-id="9d725-211">**IIS**</span><span class="sxs-lookup"><span data-stu-id="9d725-211">**IIS**</span></span>

   * <span data-ttu-id="9d725-212">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="9d725-212">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="9d725-213">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="9d725-213">**IIS Express**</span></span>

   * <span data-ttu-id="9d725-214">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="9d725-214">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="9d725-215">組態</span><span class="sxs-lookup"><span data-stu-id="9d725-215">Configuration</span></span>

<span data-ttu-id="9d725-216">**IIS**</span><span class="sxs-lookup"><span data-stu-id="9d725-216">**IIS**</span></span>

   * <span data-ttu-id="9d725-217">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="9d725-217">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="9d725-218">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="9d725-218">**IIS Express**</span></span>

   * <span data-ttu-id="9d725-219">.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="9d725-219">.vs\config\applicationHost.config</span></span>

<span data-ttu-id="9d725-220">您可以搜尋*aspnetcore.dll*中*applicationHost.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="9d725-220">You can search for *aspnetcore.dll* in the *applicationHost.config* file.</span></span> <span data-ttu-id="9d725-221">IIS express， *applicationHost.config*根據預設，檔案不存在。</span><span class="sxs-lookup"><span data-stu-id="9d725-221">For IIS Express, the *applicationHost.config* file won't exist by default.</span></span> <span data-ttu-id="9d725-222">檔案建立在*{應用程式根目錄}\.vs\config*當您啟動任何 web 應用程式專案中的 Visual Studio 方案。</span><span class="sxs-lookup"><span data-stu-id="9d725-222">The file is created at *{application root}\.vs\config* when you start any web application project in the Visual Studio solution.</span></span>