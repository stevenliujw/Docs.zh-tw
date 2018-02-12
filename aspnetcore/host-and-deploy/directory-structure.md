---
title: "ASP.NET Core 目錄結構"
author: guardrex
description: "請參閱已發行的 ASP.NET Core 應用程式的目錄結構。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/directory-structure
ms.openlocfilehash: 55e1e0dac32609446243098dbb4a4373f06b4212
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="directory-structure-of-published-aspnet-core-apps"></a><span data-ttu-id="45d79-103">已發行的 ASP.NET Core 應用程式的目錄結構</span><span class="sxs-lookup"><span data-stu-id="45d79-103">Directory structure of published ASP.NET Core apps</span></span>

<span data-ttu-id="45d79-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="45d79-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="45d79-105">在 ASP.NET Core，應用程式目錄，*發行*，組成應用程式檔案、 組態檔、 靜態資產、 封裝和執行階段 （適用於獨立的應用程式）。</span><span class="sxs-lookup"><span data-stu-id="45d79-105">In ASP.NET Core, the application directory, *publish*, is comprised of application files, config files, static assets, packages, and the runtime (for self-contained apps).</span></span>

| <span data-ttu-id="45d79-106">應用程式類型</span><span class="sxs-lookup"><span data-stu-id="45d79-106">App Type</span></span>                       | <span data-ttu-id="45d79-107">目錄結構</span><span class="sxs-lookup"><span data-stu-id="45d79-107">Directory Structure</span></span> |
| ------------------------------ | ------------------- |
| <span data-ttu-id="45d79-108">Framework 相依的部署</span><span class="sxs-lookup"><span data-stu-id="45d79-108">Framework-dependent Deployment</span></span> | <ul><li><span data-ttu-id="45d79-109">publish\*</span><span class="sxs-lookup"><span data-stu-id="45d79-109">publish\*</span></span><ul><li><span data-ttu-id="45d79-110">記錄檔\*（如果包含在 publishOptions）</span><span class="sxs-lookup"><span data-stu-id="45d79-110">logs\* (if included in publishOptions)</span></span></li><li><span data-ttu-id="45d79-111">refs\*</span><span class="sxs-lookup"><span data-stu-id="45d79-111">refs\*</span></span></li><li><span data-ttu-id="45d79-112">執行階段\*</span><span class="sxs-lookup"><span data-stu-id="45d79-112">runtimes\*</span></span></li><li><span data-ttu-id="45d79-113">檢視\*（如果包含在 publishOptions）</span><span class="sxs-lookup"><span data-stu-id="45d79-113">Views\* (if included in publishOptions)</span></span></li><li><span data-ttu-id="45d79-114">wwwroot\* （如果包含在 publishOptions）</span><span class="sxs-lookup"><span data-stu-id="45d79-114">wwwroot\* (if included in publishOptions)</span></span></li><li><span data-ttu-id="45d79-115">.dll 檔案</span><span class="sxs-lookup"><span data-stu-id="45d79-115">.dll files</span></span></li><li><span data-ttu-id="45d79-116">myapp.deps.json</span><span class="sxs-lookup"><span data-stu-id="45d79-116">myapp.deps.json</span></span></li><li><span data-ttu-id="45d79-117">myapp.dll</span><span class="sxs-lookup"><span data-stu-id="45d79-117">myapp.dll</span></span></li><li><span data-ttu-id="45d79-118">myapp.pdb</span><span class="sxs-lookup"><span data-stu-id="45d79-118">myapp.pdb</span></span></li><li><span data-ttu-id="45d79-119">myapp。PrecompiledViews.dll （如果先行編譯 Razor 檢視）</span><span class="sxs-lookup"><span data-stu-id="45d79-119">myapp.PrecompiledViews.dll (if precompiling Razor Views)</span></span></li><li><span data-ttu-id="45d79-120">myapp。PrecompiledViews.pdb （如果先行編譯 Razor 檢視）</span><span class="sxs-lookup"><span data-stu-id="45d79-120">myapp.PrecompiledViews.pdb (if precompiling Razor Views)</span></span></li><li><span data-ttu-id="45d79-121">myapp.runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="45d79-121">myapp.runtimeconfig.json</span></span></li><li><span data-ttu-id="45d79-122">（如果包含在 publishOptions） 的 web.config</span><span class="sxs-lookup"><span data-stu-id="45d79-122">web.config (if included in publishOptions)</span></span></li></ul></li></ul> |
| <span data-ttu-id="45d79-123">獨立的部署</span><span class="sxs-lookup"><span data-stu-id="45d79-123">Self-contained Deployment</span></span>      | <ul><li><span data-ttu-id="45d79-124">publish\*</span><span class="sxs-lookup"><span data-stu-id="45d79-124">publish\*</span></span><ul><li><span data-ttu-id="45d79-125">記錄檔\*（如果包含在 publishOptions）</span><span class="sxs-lookup"><span data-stu-id="45d79-125">logs\* (if included in publishOptions)</span></span></li><li><span data-ttu-id="45d79-126">refs\*</span><span class="sxs-lookup"><span data-stu-id="45d79-126">refs\*</span></span></li><li><span data-ttu-id="45d79-127">檢視\*（如果包含在 publishOptions）</span><span class="sxs-lookup"><span data-stu-id="45d79-127">Views\* (if included in publishOptions)</span></span></li><li><span data-ttu-id="45d79-128">wwwroot\* （如果包含在 publishOptions）</span><span class="sxs-lookup"><span data-stu-id="45d79-128">wwwroot\* (if included in publishOptions)</span></span></li><li><span data-ttu-id="45d79-129">.dll 檔案</span><span class="sxs-lookup"><span data-stu-id="45d79-129">.dll files</span></span></li><li><span data-ttu-id="45d79-130">myapp.deps.json</span><span class="sxs-lookup"><span data-stu-id="45d79-130">myapp.deps.json</span></span></li><li><span data-ttu-id="45d79-131">myapp.exe</span><span class="sxs-lookup"><span data-stu-id="45d79-131">myapp.exe</span></span></li><li><span data-ttu-id="45d79-132">myapp.pdb</span><span class="sxs-lookup"><span data-stu-id="45d79-132">myapp.pdb</span></span></li><li><span data-ttu-id="45d79-133">myapp。PrecompiledViews.dll （如果先行編譯 Razor 檢視）</span><span class="sxs-lookup"><span data-stu-id="45d79-133">myapp.PrecompiledViews.dll (if precompiling Razor Views)</span></span></li><li><span data-ttu-id="45d79-134">myapp。PrecompiledViews.pdb （如果先行編譯 Razor 檢視）</span><span class="sxs-lookup"><span data-stu-id="45d79-134">myapp.PrecompiledViews.pdb (if precompiling Razor Views)</span></span></li><li><span data-ttu-id="45d79-135">myapp.runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="45d79-135">myapp.runtimeconfig.json</span></span></li><li><span data-ttu-id="45d79-136">（如果包含在 publishOptions） 的 web.config</span><span class="sxs-lookup"><span data-stu-id="45d79-136">web.config (if included in publishOptions)</span></span></li></ul></li></ul> |
<span data-ttu-id="45d79-137">\*表示目錄</span><span class="sxs-lookup"><span data-stu-id="45d79-137">\* Indicates a directory</span></span>

<span data-ttu-id="45d79-138">內容*發行*目錄代表*內容的根路徑*，也稱為*應用程式基底路徑*的部署。</span><span class="sxs-lookup"><span data-stu-id="45d79-138">The contents of the *publish* directory represents the *content root path*, also called the *application base path*, of the deployment.</span></span> <span data-ttu-id="45d79-139">若要指定任何名稱*發行*部署中的目錄，它的位置做為託管的應用程式伺服器的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="45d79-139">Whatever name is given to the *publish* directory in the deployment, its location serves as the server's physical path to the hosted application.</span></span> <span data-ttu-id="45d79-140">*Wwwroot*目錄中，如果有的話，只包含靜態資產。</span><span class="sxs-lookup"><span data-stu-id="45d79-140">The *wwwroot* directory, if present, only contains static assets.</span></span> <span data-ttu-id="45d79-141">*記錄*可能藉由建立專案中加入部署中包含目錄`<Target>`項目，如下所示，以您*.csproj*檔案或實體上建立目錄伺服器。</span><span class="sxs-lookup"><span data-stu-id="45d79-141">The *logs* directory may be included in the deployment by creating it in the project and adding the `<Target>` element shown below to your *.csproj* file or by physically creating the directory on the server.</span></span>

```xml
<Target Name="CreateLogsFolder" AfterTargets="Publish">
  <MakeDir Directories="$(PublishDir)Logs" 
           Condition="!Exists('$(PublishDir)Logs')" />
  <WriteLinesToFile File="$(PublishDir)Logs\.log" 
                    Lines="Generated file" 
                    Overwrite="True" 
                    Condition="!Exists('$(PublishDir)Logs\.log')" />
</Target>
```

<span data-ttu-id="45d79-142">`<MakeDir>`項目會建立空*記錄*中發行的輸出資料夾。</span><span class="sxs-lookup"><span data-stu-id="45d79-142">The `<MakeDir>` element creates an empty *Logs* folder in the published output.</span></span> <span data-ttu-id="45d79-143">項目使用`PublishDir`屬性來判斷建立此資料夾的目標位置。</span><span class="sxs-lookup"><span data-stu-id="45d79-143">The element uses the `PublishDir` property to determine the target location for creating the folder.</span></span> <span data-ttu-id="45d79-144">數種部署方法，例如 Web Deploy，請在部署期間，略過空資料夾。</span><span class="sxs-lookup"><span data-stu-id="45d79-144">Several deployment methods, such as Web Deploy, skip empty folders during deployment.</span></span> <span data-ttu-id="45d79-145">`<WriteLinesToFile>`項目會產生的檔案中*記錄*資料夾中，以確保部署到伺服器的資料夾。</span><span class="sxs-lookup"><span data-stu-id="45d79-145">The `<WriteLinesToFile>` element generates a file in the *Logs* folder, which guarantees deployment of the folder to the server.</span></span> <span data-ttu-id="45d79-146">請注意是否工作者處理序不具有寫入存取權的目標資料夾可能仍然無法建立資料夾。</span><span class="sxs-lookup"><span data-stu-id="45d79-146">Note that folder creation may still fail if the worker process doesn't have write access to the target folder.</span></span>

<span data-ttu-id="45d79-147">部署目錄中需要讀取/執行權限，雖然*記錄*目錄需要讀取/寫入權限。</span><span class="sxs-lookup"><span data-stu-id="45d79-147">The deployment directory requires Read/Execute permissions, while the *Logs* directory requires Read/Write permissions.</span></span> <span data-ttu-id="45d79-148">其他目錄將會寫入資產需要讀取/寫入權限。</span><span class="sxs-lookup"><span data-stu-id="45d79-148">Additional directories where assets will be written require Read/Write permissions.</span></span>