---
title: "檢視元件"
author: rick-anderson
description: "檢視元件是任何位置有可重複使用的呈現邏輯。"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-components
ms.openlocfilehash: 2d93dcee102009661af708b9a9066e8af0bdbb17
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="view-components"></a><span data-ttu-id="ce68d-103">檢視元件</span><span class="sxs-lookup"><span data-stu-id="ce68d-103">View components</span></span>

<span data-ttu-id="ce68d-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ce68d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ce68d-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ce68d-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="introducing-view-components"></a><span data-ttu-id="ce68d-106">介紹檢視元件</span><span class="sxs-lookup"><span data-stu-id="ce68d-106">Introducing view components</span></span>

<span data-ttu-id="ce68d-107">新的 ASP.NET Core MVC 中，以檢視元件類似於部分檢視，但它們是更為強大。</span><span class="sxs-lookup"><span data-stu-id="ce68d-107">New to ASP.NET Core MVC, view components are similar to partial views, but they are much more powerful.</span></span> <span data-ttu-id="ce68d-108">檢視元件不使用模型繫結，並僅相依於呼叫它時，您所提供的資料。</span><span class="sxs-lookup"><span data-stu-id="ce68d-108">View components don’t use model binding, and only depend on the data you provide when calling into it.</span></span> <span data-ttu-id="ce68d-109">檢視元件：</span><span class="sxs-lookup"><span data-stu-id="ce68d-109">A view component:</span></span>

* <span data-ttu-id="ce68d-110">呈現區塊 (chunk)，而不是完整的回應</span><span class="sxs-lookup"><span data-stu-id="ce68d-110">Renders a chunk rather than a whole response</span></span>
* <span data-ttu-id="ce68d-111">包含的考量相同隔離-和控制器和檢視之間的可測試性優點</span><span class="sxs-lookup"><span data-stu-id="ce68d-111">Includes the same separation-of-concerns and testability benefits found between a controller and view</span></span>
* <span data-ttu-id="ce68d-112">可以具有參數和商務邏輯</span><span class="sxs-lookup"><span data-stu-id="ce68d-112">Can have parameters and business logic</span></span>
* <span data-ttu-id="ce68d-113">這通常是從版面配置頁面叫用</span><span class="sxs-lookup"><span data-stu-id="ce68d-113">Is typically invoked from a layout page</span></span>

<span data-ttu-id="ce68d-114">檢視元件是任何位置有可重複使用的呈現邏輯太複雜，部分檢視，例如：</span><span class="sxs-lookup"><span data-stu-id="ce68d-114">View components are intended anywhere you have reusable rendering logic that is too complex for a partial view, such as:</span></span>

* <span data-ttu-id="ce68d-115">動態導覽功能表</span><span class="sxs-lookup"><span data-stu-id="ce68d-115">Dynamic navigation menus</span></span>
* <span data-ttu-id="ce68d-116">標記雲端 （其中查詢資料庫）</span><span class="sxs-lookup"><span data-stu-id="ce68d-116">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="ce68d-117">登入 面板</span><span class="sxs-lookup"><span data-stu-id="ce68d-117">Login panel</span></span>
* <span data-ttu-id="ce68d-118">購物車</span><span class="sxs-lookup"><span data-stu-id="ce68d-118">Shopping cart</span></span>
* <span data-ttu-id="ce68d-119">最近已發行的文章</span><span class="sxs-lookup"><span data-stu-id="ce68d-119">Recently published articles</span></span>
* <span data-ttu-id="ce68d-120">典型的部落格上的 [資訊看板] 內容</span><span class="sxs-lookup"><span data-stu-id="ce68d-120">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="ce68d-121">登入面板，將每一頁上轉譯，並示範登出或登入，根據使用者的狀態中的記錄檔連結</span><span class="sxs-lookup"><span data-stu-id="ce68d-121">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="ce68d-122">檢視元件是由兩個部分所組成： 類別 (通常衍生自[ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) 並將它傳回 （通常檢視）。</span><span class="sxs-lookup"><span data-stu-id="ce68d-122">A view component consists of two parts: the class (typically derived from [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="ce68d-123">控制站，像是檢視元件可以 POCO，但大部分的開發人員會想要充分利用的方法和屬性可由衍生自`ViewComponent`。</span><span class="sxs-lookup"><span data-stu-id="ce68d-123">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="ce68d-124">建立檢視的元件</span><span class="sxs-lookup"><span data-stu-id="ce68d-124">Creating a view component</span></span>

<span data-ttu-id="ce68d-125">本節包含高階的需求，建立檢視的元件。</span><span class="sxs-lookup"><span data-stu-id="ce68d-125">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="ce68d-126">本文稍後我們會檢查每個步驟的詳細資料，並建立檢視的元件。</span><span class="sxs-lookup"><span data-stu-id="ce68d-126">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="ce68d-127">檢視元件類別</span><span class="sxs-lookup"><span data-stu-id="ce68d-127">The view component class</span></span>

<span data-ttu-id="ce68d-128">您可以建立檢視元件類別由下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="ce68d-128">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="ce68d-129">衍生自*ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="ce68d-129">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="ce68d-130">裝飾的類別`[ViewComponent]`屬性，或衍生自類別`[ViewComponent]`屬性</span><span class="sxs-lookup"><span data-stu-id="ce68d-130">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="ce68d-131">建立的類別名稱結尾的後置詞*ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="ce68d-131">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="ce68d-132">控制站，像是檢視元件必須是公用、 以非巢狀，和非抽象類別。</span><span class="sxs-lookup"><span data-stu-id="ce68d-132">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="ce68d-133">檢視元件名稱是"ViewComponent"後置詞，移除類別名稱。</span><span class="sxs-lookup"><span data-stu-id="ce68d-133">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="ce68d-134">它也可以明確地指定使用`ViewComponentAttribute.Name`屬性。</span><span class="sxs-lookup"><span data-stu-id="ce68d-134">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="ce68d-135">檢視元件類別：</span><span class="sxs-lookup"><span data-stu-id="ce68d-135">A view component class:</span></span>

* <span data-ttu-id="ce68d-136">完全支援建構函式[相依性插入](../../fundamentals/dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="ce68d-136">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="ce68d-137">不接受部分中控制站的生命週期，這表示您無法使用[篩選](../controllers/filters.md)中檢視元件</span><span class="sxs-lookup"><span data-stu-id="ce68d-137">Does not take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="ce68d-138">檢視元件方法</span><span class="sxs-lookup"><span data-stu-id="ce68d-138">View component methods</span></span>

<span data-ttu-id="ce68d-139">檢視元件會定義其邏輯中的`InvokeAsync`方法會傳回`IViewComponentResult`。</span><span class="sxs-lookup"><span data-stu-id="ce68d-139">A view component defines its logic in an `InvokeAsync` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="ce68d-140">參數直接來自檢視元件，不是從模型繫結的引動過程。</span><span class="sxs-lookup"><span data-stu-id="ce68d-140">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="ce68d-141">檢視元件時，永遠不會直接處理要求。</span><span class="sxs-lookup"><span data-stu-id="ce68d-141">A view component never directly handles a request.</span></span> <span data-ttu-id="ce68d-142">通常，檢視元件初始化模型，並將其傳遞到檢視中，藉由呼叫`View`方法。</span><span class="sxs-lookup"><span data-stu-id="ce68d-142">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="ce68d-143">在 [摘要] 檢視元件方法：</span><span class="sxs-lookup"><span data-stu-id="ce68d-143">In summary, view component methods:</span></span>

* <span data-ttu-id="ce68d-144">定義`InvokeAsync`方法會傳回`IViewComponentResult`</span><span class="sxs-lookup"><span data-stu-id="ce68d-144">Define an `InvokeAsync` method that returns an `IViewComponentResult`</span></span>
* <span data-ttu-id="ce68d-145">通常初始化模型，並將其傳遞到檢視中，藉由呼叫`ViewComponent``View`方法</span><span class="sxs-lookup"><span data-stu-id="ce68d-145">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method</span></span>
* <span data-ttu-id="ce68d-146">參數會來自呼叫的方法，而不是 HTTP、 沒有模型繫結</span><span class="sxs-lookup"><span data-stu-id="ce68d-146">Parameters come from the calling method, not HTTP, there is no model binding</span></span>
* <span data-ttu-id="ce68d-147">會無法連線到做為 HTTP 端點，即會叫用您的程式碼 （通常是在檢視中）。</span><span class="sxs-lookup"><span data-stu-id="ce68d-147">Are not reachable directly as an HTTP endpoint, they are invoked from your code (usually in a view).</span></span> <span data-ttu-id="ce68d-148">檢視元件永遠不會處理要求</span><span class="sxs-lookup"><span data-stu-id="ce68d-148">A view component never handles a request</span></span>
* <span data-ttu-id="ce68d-149">多載簽章，而不是從目前的 HTTP 要求的任何詳細資料</span><span class="sxs-lookup"><span data-stu-id="ce68d-149">Are overloaded on the signature rather than any details from the current HTTP request</span></span>

### <a name="view-search-path"></a><span data-ttu-id="ce68d-150">檢視搜尋路徑</span><span class="sxs-lookup"><span data-stu-id="ce68d-150">View search path</span></span>

<span data-ttu-id="ce68d-151">執行階段會搜尋下列路徑中的檢視：</span><span class="sxs-lookup"><span data-stu-id="ce68d-151">The runtime searches for the view in the following paths:</span></span>

   * <span data-ttu-id="ce68d-152">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span><span class="sxs-lookup"><span data-stu-id="ce68d-152">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span></span>
   * <span data-ttu-id="ce68d-153">Views/Shared/Components/\<view_component_name>/\<view_name></span><span class="sxs-lookup"><span data-stu-id="ce68d-153">Views/Shared/Components/\<view_component_name>/\<view_name></span></span>

<span data-ttu-id="ce68d-154">檢視元件中預設的檢視名稱是*預設*，這表示您的檢視檔案將通常會命名為*Default.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="ce68d-154">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="ce68d-155">建立元件檢視結果時，或呼叫時，您可以指定不同的檢視名稱`View`方法。</span><span class="sxs-lookup"><span data-stu-id="ce68d-155">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="ce68d-156">我們建議您檢視檔案名稱*Default.cshtml*並用*檢視/共用元件/\<view_component_name > /\<view_name >*路徑。</span><span class="sxs-lookup"><span data-stu-id="ce68d-156">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/\<view_component_name>/\<view_name>* path.</span></span> <span data-ttu-id="ce68d-157">`PriorityList`檢視此範例中使用的元件會使用*Views/Shared/Components/PriorityList/Default.cshtml*檢視元件檢視。</span><span class="sxs-lookup"><span data-stu-id="ce68d-157">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="ce68d-158">叫用的檢視元件</span><span class="sxs-lookup"><span data-stu-id="ce68d-158">Invoking a view component</span></span>

<span data-ttu-id="ce68d-159">若要使用的檢視元件，請呼叫下列檢視內：</span><span class="sxs-lookup"><span data-stu-id="ce68d-159">To use the view component, call the following inside a view:</span></span>

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

<span data-ttu-id="ce68d-160">參數會傳遞至`InvokeAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="ce68d-160">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="ce68d-161">`PriorityList`開發文件中的檢視元件會從叫用*Views/Todo/Index.cshtml*檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="ce68d-161">The `PriorityList` view component developed in the article is invoked from the *Views/Todo/Index.cshtml* view file.</span></span> <span data-ttu-id="ce68d-162">下列程式碼，在`InvokeAsync`具有兩個參數呼叫方法：</span><span class="sxs-lookup"><span data-stu-id="ce68d-162">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="ce68d-163">叫用的檢視元件，以標記協助程式</span><span class="sxs-lookup"><span data-stu-id="ce68d-163">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="ce68d-164">針對 ASP.NET Core 1.1 和更新版本，您可以叫用檢視元件做為[標記協助程式](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="ce68d-164">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="ce68d-165">依照 pascal 命名法的大小寫慣例類別和方法的參數標記協助程式會轉譯成其[降低 kebab 案例](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101)。</span><span class="sxs-lookup"><span data-stu-id="ce68d-165">Pascal-cased class and method parameters for Tag Helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="ce68d-166">標記協助程式叫用檢視元件會使用`<vc></vc>`項目。</span><span class="sxs-lookup"><span data-stu-id="ce68d-166">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="ce68d-167">檢視元件的指定方式如下：</span><span class="sxs-lookup"><span data-stu-id="ce68d-167">The view component is specified as follows:</span></span>

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="ce68d-168">注意： 若要使用檢視元件做為標記協助程式，您必須註冊包含檢視元件使用的組件`@addTagHelper`指示詞。</span><span class="sxs-lookup"><span data-stu-id="ce68d-168">Note: In order to use a View Component as a Tag Helper, you must register the assembly containing the View Component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="ce68d-169">例如，如果您檢視的元件稱為"M"的組件中，新增下列指示詞，以`_ViewImports.cshtml`檔案：</span><span class="sxs-lookup"><span data-stu-id="ce68d-169">For example, if your View Component is in an assembly called "MyWebApp", add the following directive to the `_ViewImports.cshtml` file:</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="ce68d-170">您可以註冊檢視元件做為標記協助程式參考檢視元件的任何檔案。</span><span class="sxs-lookup"><span data-stu-id="ce68d-170">You can register a View Component as a Tag Helper to any file that references the View Component.</span></span> <span data-ttu-id="ce68d-171">請參閱[管理標記協助程式範圍](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope)如需有關如何註冊標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="ce68d-171">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="ce68d-172">`InvokeAsync`本教學課程中使用的方法：</span><span class="sxs-lookup"><span data-stu-id="ce68d-172">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="ce68d-173">在標記協助程式標記：</span><span class="sxs-lookup"><span data-stu-id="ce68d-173">In Tag Helper markup:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="ce68d-174">在上述範例`PriorityList`檢視元件會變成`priority-list`。</span><span class="sxs-lookup"><span data-stu-id="ce68d-174">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="ce68d-175">檢視元件的參數會在小寫 kebab 傳遞做為屬性。</span><span class="sxs-lookup"><span data-stu-id="ce68d-175">The parameters to the view component are passed as attributes in lower kebab case.</span></span>

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="ce68d-176">叫用的檢視元件，直接從控制器</span><span class="sxs-lookup"><span data-stu-id="ce68d-176">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="ce68d-177">通常會從檢視中，會叫用檢視元件，但您可以直接從控制器方法叫用它們。</span><span class="sxs-lookup"><span data-stu-id="ce68d-177">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="ce68d-178">檢視元件不會定義端點，如控制站，您可以輕鬆地實作控制器動作傳回的內容`ViewComponentResult`。</span><span class="sxs-lookup"><span data-stu-id="ce68d-178">While view components do not define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="ce68d-179">在此範例中，檢視元件稱為直接從控制器：</span><span class="sxs-lookup"><span data-stu-id="ce68d-179">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="ce68d-180">逐步解說： 建立簡單的檢視元件</span><span class="sxs-lookup"><span data-stu-id="ce68d-180">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="ce68d-181">[下載](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample)、 建置和測試的起始程式碼。</span><span class="sxs-lookup"><span data-stu-id="ce68d-181">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="ce68d-182">它是簡單的專案與`Todo`控制站，會顯示一份*Todo*項目。</span><span class="sxs-lookup"><span data-stu-id="ce68d-182">It's a simple project with a `Todo` controller that displays a list of *Todo* items.</span></span>

![ToDos 的清單](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="ce68d-184">加入 ViewComponent 類別</span><span class="sxs-lookup"><span data-stu-id="ce68d-184">Add a ViewComponent class</span></span>

<span data-ttu-id="ce68d-185">建立*ViewComponents*資料夾並加入下列`PriorityListViewComponent`類別：</span><span class="sxs-lookup"><span data-stu-id="ce68d-185">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="ce68d-186">附註的程式碼：</span><span class="sxs-lookup"><span data-stu-id="ce68d-186">Notes on the code:</span></span>

* <span data-ttu-id="ce68d-187">檢視元件類別可以包含在**任何**專案資料夾中的。</span><span class="sxs-lookup"><span data-stu-id="ce68d-187">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="ce68d-188">因為的類別名稱 PriorityList**ViewComponent**結束的尾碼**ViewComponent**，參考從檢視表的類別元件時，執行階段會使用字串"PriorityList"。</span><span class="sxs-lookup"><span data-stu-id="ce68d-188">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="ce68d-189">我將說明的詳細更新版本。</span><span class="sxs-lookup"><span data-stu-id="ce68d-189">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="ce68d-190">`[ViewComponent]`屬性可以變更用來參考的檢視元件的名稱。</span><span class="sxs-lookup"><span data-stu-id="ce68d-190">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="ce68d-191">例如，我們無法已命名類別`XYZ`和套用`ViewComponent`屬性：</span><span class="sxs-lookup"><span data-stu-id="ce68d-191">For example, we could have named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="ce68d-192">`[ViewComponent]`上述的屬性會告知檢視元件選擇器，使用名稱`PriorityList`尋找與元件，並從檢視表參考類別元件時，請使用字串"PriorityList"相關聯的檢視時。</span><span class="sxs-lookup"><span data-stu-id="ce68d-192">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="ce68d-193">我將說明的詳細更新版本。</span><span class="sxs-lookup"><span data-stu-id="ce68d-193">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="ce68d-194">這個元件會使用[相依性插入](../../fundamentals/dependency-injection.md)若要使用的資料內容。</span><span class="sxs-lookup"><span data-stu-id="ce68d-194">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="ce68d-195">`InvokeAsync`會公開從一個檢視，以及它可以呼叫這些方法可以採用任意數目的引數。</span><span class="sxs-lookup"><span data-stu-id="ce68d-195">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="ce68d-196">`InvokeAsync`方法會傳回一組`ToDo`符合的項目`isDone`和`maxPriority`參數。</span><span class="sxs-lookup"><span data-stu-id="ce68d-196">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="ce68d-197">建立檢視元件 Razor 檢視</span><span class="sxs-lookup"><span data-stu-id="ce68d-197">Create the view component Razor view</span></span>

* <span data-ttu-id="ce68d-198">建立*檢視/共用元件*資料夾。</span><span class="sxs-lookup"><span data-stu-id="ce68d-198">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="ce68d-199">此資料夾**必須**命名為*元件*。</span><span class="sxs-lookup"><span data-stu-id="ce68d-199">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="ce68d-200">建立*檢視/共用/元件/PriorityList*資料夾。</span><span class="sxs-lookup"><span data-stu-id="ce68d-200">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="ce68d-201">此資料夾名稱必須符合檢視元件類別的名稱或減去後置詞的類別名稱 (如果我們遵循慣例，並使用*ViewComponent*中的類別名稱後置字元)。</span><span class="sxs-lookup"><span data-stu-id="ce68d-201">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="ce68d-202">如果您使用`ViewComponent`屬性，以符合指定的屬性，會需要類別名稱。</span><span class="sxs-lookup"><span data-stu-id="ce68d-202">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="ce68d-203">建立*Views/Shared/Components/PriorityList/Default.cshtml* Razor 檢視：[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="ce68d-203">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>
    
   <span data-ttu-id="ce68d-204">Razor 檢視採用一份`TodoItem`並加以顯示。</span><span class="sxs-lookup"><span data-stu-id="ce68d-204">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="ce68d-205">如果檢視元件`InvokeAsync`方法未通過 （我們如範例所示），檢視名稱*預設*依慣例用於檢視表名稱。</span><span class="sxs-lookup"><span data-stu-id="ce68d-205">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="ce68d-206">稍後在教學課程中，我將示範如何將檢視的名稱。</span><span class="sxs-lookup"><span data-stu-id="ce68d-206">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="ce68d-207">若要覆寫特定控制器的預設樣式，加入檢視到控制器的特定檢視資料夾 (例如*Views/Todo/Components/PriorityList/Default.cshtml)*。</span><span class="sxs-lookup"><span data-stu-id="ce68d-207">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/Todo/Components/PriorityList/Default.cshtml)*.</span></span>
    
    <span data-ttu-id="ce68d-208">如果控制器專用的檢視元件，您可以將它加入控制器專用資料夾 (*Views/Todo/Components/PriorityList/Default.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="ce68d-208">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/Todo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="ce68d-209">新增`div`包含呼叫的底部優先順序清單元件*Views/Todo/index.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="ce68d-209">Add a `div` containing a call to the priority list component to the bottom of the *Views/Todo/index.cshtml* file:</span></span>

    [!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="ce68d-210">標記`@await Component.InvokeAsync`顯示的語法呼叫檢視元件。</span><span class="sxs-lookup"><span data-stu-id="ce68d-210">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="ce68d-211">第一個引數是我們想要叫用，或是呼叫之元件的名稱。</span><span class="sxs-lookup"><span data-stu-id="ce68d-211">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="ce68d-212">後續的參數會傳遞至元件。</span><span class="sxs-lookup"><span data-stu-id="ce68d-212">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="ce68d-213">`InvokeAsync`可以接受任意數目的引數。</span><span class="sxs-lookup"><span data-stu-id="ce68d-213">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="ce68d-214">測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="ce68d-214">Test the app.</span></span> <span data-ttu-id="ce68d-215">下圖顯示 ToDo 清單和優先權的項目：</span><span class="sxs-lookup"><span data-stu-id="ce68d-215">The following image shows the ToDo list and the priority items:</span></span>

![todo 清單和優先順序的項目](view-components/_static/pi.png)

<span data-ttu-id="ce68d-217">您也可以直接從控制器呼叫檢視元件：</span><span class="sxs-lookup"><span data-stu-id="ce68d-217">You can also call the view component directly from the controller:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![從 IndexVC 動作的優先順序項目](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="ce68d-219">指定檢視表名稱</span><span class="sxs-lookup"><span data-stu-id="ce68d-219">Specifying a view name</span></span>

<span data-ttu-id="ce68d-220">若要指定非預設的檢視，在某些情況下，可能需要複雜的檢視元件。</span><span class="sxs-lookup"><span data-stu-id="ce68d-220">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="ce68d-221">下列程式碼示範如何指定"PVC 」 檢視`InvokeAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="ce68d-221">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="ce68d-222">更新`InvokeAsync`方法中的`PriorityListViewComponent`類別。</span><span class="sxs-lookup"><span data-stu-id="ce68d-222">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="ce68d-223">複製*Views/Shared/Components/PriorityList/Default.cshtml*檔案到名為檢視*Views/Shared/Components/PriorityList/PVC.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="ce68d-223">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="ce68d-224">新增標題，以表示正在使用 PVC 檢視中。</span><span class="sxs-lookup"><span data-stu-id="ce68d-224">Add a heading to indicate the PVC view is being used.</span></span>

[!code-cshtml[Main](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="ce68d-225">更新*Views/TodoList/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ce68d-225">Update *Views/TodoList/Index.cshtml*:</span></span>

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="ce68d-226">執行應用程式，並確認 PVC 檢視。</span><span class="sxs-lookup"><span data-stu-id="ce68d-226">Run the app and verify PVC view.</span></span>

![優先順序檢視元件](view-components/_static/pvc.png)

<span data-ttu-id="ce68d-228">如果 PVC 檢視不會轉譯，請確認您要呼叫的檢視元件，優先順序為 4 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="ce68d-228">If the PVC view is not rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="ce68d-229">請檢查檢視路徑</span><span class="sxs-lookup"><span data-stu-id="ce68d-229">Examine the view path</span></span>

* <span data-ttu-id="ce68d-230">為三個或更小，則不會傳回 priority 檢視，變更優先順序參數。</span><span class="sxs-lookup"><span data-stu-id="ce68d-230">Change the priority parameter to three or less so the priority view is not returned.</span></span>
* <span data-ttu-id="ce68d-231">暫時重新命名*Views/Todo/Components/PriorityList/Default.cshtml*至*1Default.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="ce68d-231">Temporarily rename the *Views/Todo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="ce68d-232">測試應用程式，您會收到下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="ce68d-232">Test the app, you'll get the following error:</span></span>

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' was not found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="ce68d-233">複製*Views/Todo/Components/PriorityList/1Default.cshtml*至*Views/Shared/Components/PriorityList/Default.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="ce68d-233">Copy *Views/Todo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="ce68d-234">加入至某些標記*共用*Todo 檢視元件檢視，來指示檢視取自*共用*資料夾。</span><span class="sxs-lookup"><span data-stu-id="ce68d-234">Add some markup to the *Shared* Todo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="ce68d-235">測試**共用**元件檢視。</span><span class="sxs-lookup"><span data-stu-id="ce68d-235">Test the **Shared** component view.</span></span>

![共用元件檢視 ToDo 輸出](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a><span data-ttu-id="ce68d-237">避免 magic 字串</span><span class="sxs-lookup"><span data-stu-id="ce68d-237">Avoiding magic strings</span></span>

<span data-ttu-id="ce68d-238">如果您想編譯時間安全性，您可以硬式編碼的檢視元件名稱取代類別名稱。</span><span class="sxs-lookup"><span data-stu-id="ce68d-238">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="ce68d-239">建立不含"ViewComponent"尾碼的檢視元件：</span><span class="sxs-lookup"><span data-stu-id="ce68d-239">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="ce68d-240">新增`using`陳述式，您 Razor 檢視檔案，並使用`nameof`運算子：</span><span class="sxs-lookup"><span data-stu-id="ce68d-240">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,33-)]

## <a name="additional-resources"></a><span data-ttu-id="ce68d-241">其他資源</span><span class="sxs-lookup"><span data-stu-id="ce68d-241">Additional Resources</span></span>

* [<span data-ttu-id="ce68d-242">在檢視中插入相依性</span><span class="sxs-lookup"><span data-stu-id="ce68d-242">Dependency injection into views</span></span>](dependency-injection.md)