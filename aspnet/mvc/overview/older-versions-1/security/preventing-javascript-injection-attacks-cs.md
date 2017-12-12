---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-cs
title: "防止 JavaScript 插入式攻擊 (C#) |Microsoft 文件"
author: StephenWalther
description: "防止 JavaScript 插入式攻擊和跨網站指令碼處理攻擊發生給您。 在此教學課程中，作者： Stephen Walther 會說明如何輕鬆地 de..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: d0136da6-81a4-4815-b002-baa84744c09e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-cs
msc.type: authoredcontent
ms.openlocfilehash: 67f53162cb1bb0771d632ba7a3f5960db00e2744
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="preventing-javascript-injection-attacks-c"></a><span data-ttu-id="6d81a-104">防止 JavaScript 插入式攻擊 (C#)</span><span class="sxs-lookup"><span data-stu-id="6d81a-104">Preventing JavaScript Injection Attacks (C#)</span></span>
====================
<span data-ttu-id="6d81a-105">由[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="6d81a-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

[<span data-ttu-id="6d81a-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="6d81a-106">Download PDF</span></span>](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_CS.pdf)

> <span data-ttu-id="6d81a-107">防止 JavaScript 插入式攻擊和跨網站指令碼處理攻擊發生給您。</span><span class="sxs-lookup"><span data-stu-id="6d81a-107">Prevent JavaScript Injection Attacks and Cross-Site Scripting Attacks from happening to you.</span></span> <span data-ttu-id="6d81a-108">在本教學課程中，作者： Stephen Walther 會說明如何輕鬆地擊敗這類攻擊的 HTML 編碼您的內容。</span><span class="sxs-lookup"><span data-stu-id="6d81a-108">In this tutorial, Stephen Walther explains how you can easily defeat these types of attacks by HTML encoding your content.</span></span>


<span data-ttu-id="6d81a-109">本教學課程的目標是以說明您可以在如何在 ASP.NET MVC 應用程式中防止 JavaScript 插入式攻擊。</span><span class="sxs-lookup"><span data-stu-id="6d81a-109">The goal of this tutorial is to explain how you can prevent JavaScript injection attacks in your ASP.NET MVC applications.</span></span> <span data-ttu-id="6d81a-110">本教學課程將討論兩種方法來保護您的網站 JavaScript 資料隱碼攻擊。</span><span class="sxs-lookup"><span data-stu-id="6d81a-110">This tutorial discusses two approaches to defending your website against a JavaScript injection attack.</span></span> <span data-ttu-id="6d81a-111">您了解如何防止 JavaScript 插入式攻擊編碼您顯示的資料。</span><span class="sxs-lookup"><span data-stu-id="6d81a-111">You learn how to prevent JavaScript injection attacks by encoding the data that you display.</span></span> <span data-ttu-id="6d81a-112">您也了解如何編碼的資料，您接受防止 JavaScript 插入式攻擊。</span><span class="sxs-lookup"><span data-stu-id="6d81a-112">You also learn how to prevent JavaScript injection attacks by encoding the data that you accept.</span></span>

## <a name="what-is-a-javascript-injection-attack"></a><span data-ttu-id="6d81a-113">什麼是 JavaScript 資料隱碼攻擊？</span><span class="sxs-lookup"><span data-stu-id="6d81a-113">What is a JavaScript Injection Attack?</span></span>

<span data-ttu-id="6d81a-114">每當您接受使用者輸入，並重新顯示使用者輸入，您會開啟您的網站以 JavaScript 資料隱碼攻擊。</span><span class="sxs-lookup"><span data-stu-id="6d81a-114">Whenever you accept user input and redisplay the user input, you open your website to JavaScript injection attacks.</span></span> <span data-ttu-id="6d81a-115">讓我們來檢查具象的應用程式開啟的 JavaScript 資料隱碼攻擊。</span><span class="sxs-lookup"><span data-stu-id="6d81a-115">Let's examine a concrete application that is open to JavaScript injection attacks.</span></span>

<span data-ttu-id="6d81a-116">假設您已建立客戶的意見反應網站 （請參閱圖 1）。</span><span class="sxs-lookup"><span data-stu-id="6d81a-116">Imagine that you have created a customer feedback website (see Figure 1).</span></span> <span data-ttu-id="6d81a-117">客戶可以瀏覽的網站，並使用您的產品他們經驗上輸入意見反應。</span><span class="sxs-lookup"><span data-stu-id="6d81a-117">Customers can visit the website and enter feedback on their experience using your products.</span></span> <span data-ttu-id="6d81a-118">當客戶提交意見反應時，意見反應會重新顯示在意見反應 頁面上。</span><span class="sxs-lookup"><span data-stu-id="6d81a-118">When a customer submits their feedback, the feedback is redisplayed on the feedback page.</span></span>


<span data-ttu-id="6d81a-119">[![客戶意見反應網站](preventing-javascript-injection-attacks-cs/_static/image2.png)](preventing-javascript-injection-attacks-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6d81a-119">[![Customer Feedback Website](preventing-javascript-injection-attacks-cs/_static/image2.png)](preventing-javascript-injection-attacks-cs/_static/image1.png)</span></span>

<span data-ttu-id="6d81a-120">**圖 01**： 客戶的意見反應網站 ([按一下以檢視完整大小的影像](preventing-javascript-injection-attacks-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6d81a-120">**Figure 01**: Customer Feedback Website ([Click to view full-size image](preventing-javascript-injection-attacks-cs/_static/image3.png))</span></span>


<span data-ttu-id="6d81a-121">客戶意見反應網站會使用`controller`列表 1 中。</span><span class="sxs-lookup"><span data-stu-id="6d81a-121">The customer feedback website uses the `controller` in Listing 1.</span></span> <span data-ttu-id="6d81a-122">這`controller`包含名為兩個動作`Index()`和`Create()`。</span><span class="sxs-lookup"><span data-stu-id="6d81a-122">This `controller` contains two actions named `Index()` and `Create()`.</span></span>

<span data-ttu-id="6d81a-123">**列出 1 –`HomeController.cs`**</span><span class="sxs-lookup"><span data-stu-id="6d81a-123">**Listing 1 – `HomeController.cs`**</span></span>

[!code-csharp[Main](preventing-javascript-injection-attacks-cs/samples/sample1.cs)]

<span data-ttu-id="6d81a-124">`Index()`方法顯示`Index`檢視。</span><span class="sxs-lookup"><span data-stu-id="6d81a-124">The `Index()` method displays the `Index` view.</span></span> <span data-ttu-id="6d81a-125">這個方法會傳遞所有先前的客戶意見反應給`Index`檢視擷取從資料庫 （使用 LINQ to SQL 查詢） 的意見反應。</span><span class="sxs-lookup"><span data-stu-id="6d81a-125">This method passes all of the previous customer feedback to the `Index` view by retrieving the feedback from the database (using a LINQ to SQL query).</span></span>

<span data-ttu-id="6d81a-126">`Create()`方法會建立新的意見反應項目，並將它加入至資料庫。</span><span class="sxs-lookup"><span data-stu-id="6d81a-126">The `Create()` method creates a new Feedback item and adds it to the database.</span></span> <span data-ttu-id="6d81a-127">在表單中輸入客戶的訊息傳遞至`Create()`message 參數中的方法。</span><span class="sxs-lookup"><span data-stu-id="6d81a-127">The message that the customer enters in the form is passed to the `Create()` method in the message parameter.</span></span> <span data-ttu-id="6d81a-128">建立意見反應項目，且訊息已指派給意見項目的`Message`屬性。</span><span class="sxs-lookup"><span data-stu-id="6d81a-128">A Feedback item is created and the message is assigned to the Feedback item's `Message` property.</span></span> <span data-ttu-id="6d81a-129">意見反應項目提交至資料庫`DataContext.SubmitChanges()`方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="6d81a-129">The Feedback item is submitted to the database with the `DataContext.SubmitChanges()` method call.</span></span> <span data-ttu-id="6d81a-130">最後，造訪者重新導向回到`Index`檢視顯示所有的意見反應。</span><span class="sxs-lookup"><span data-stu-id="6d81a-130">Finally, the visitor is redirected back to the `Index` view where all of the feedback is displayed.</span></span>

<span data-ttu-id="6d81a-131">`Index`檢視包含在清單 2。</span><span class="sxs-lookup"><span data-stu-id="6d81a-131">The `Index` view is contained in Listing 2.</span></span>

<span data-ttu-id="6d81a-132">**列出 2 –`Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="6d81a-132">**Listing 2 – `Index.aspx`**</span></span>

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample2.aspx)]

<span data-ttu-id="6d81a-133">`Index`檢視有兩個區段。</span><span class="sxs-lookup"><span data-stu-id="6d81a-133">The `Index` view has two sections.</span></span> <span data-ttu-id="6d81a-134">上方區段包含實際的客戶回函表單。</span><span class="sxs-lookup"><span data-stu-id="6d81a-134">The top section contains the actual customer feedback form.</span></span> <span data-ttu-id="6d81a-135">下面區段包含 For...每個迴圈，迴圈的所有先前的客戶意見反應項目，並顯示每個意見項目的 EntryDate 和訊息屬性。</span><span class="sxs-lookup"><span data-stu-id="6d81a-135">The bottom section contains a For..Each loop that loops through all of the previous customer feedback items and displays the EntryDate and Message properties for each feedback item.</span></span>

<span data-ttu-id="6d81a-136">客戶意見反應網站是一個簡單的網站。</span><span class="sxs-lookup"><span data-stu-id="6d81a-136">The customer feedback website is a simple website.</span></span> <span data-ttu-id="6d81a-137">不幸的是，網站會開啟 JavaScript 資料隱碼攻擊。</span><span class="sxs-lookup"><span data-stu-id="6d81a-137">Unfortunately, the website is open to JavaScript injection attacks.</span></span>

<span data-ttu-id="6d81a-138">假設有下列的文字輸入客戶回函表單：</span><span class="sxs-lookup"><span data-stu-id="6d81a-138">Imagine that you enter the following text into the customer feedback form:</span></span>

[!code-html[Main](preventing-javascript-injection-attacks-cs/samples/sample3.html)]

<span data-ttu-id="6d81a-139">這段文字表示的 JavaScript 指令碼，會顯示警示訊息方塊。</span><span class="sxs-lookup"><span data-stu-id="6d81a-139">This text represents a JavaScript script that displays an alert message box.</span></span> <span data-ttu-id="6d81a-140">有人將此指令碼提交至意見反應之後形成，訊息*Boo ！*任何人拜訪客戶意見反應網站將來 （請參閱圖 2） 時，會出現。</span><span class="sxs-lookup"><span data-stu-id="6d81a-140">After someone submits this script into the feedback form, the message *Boo!*will appear whenever anyone visits the customer feedback website in the future (see Figure 2).</span></span>


<span data-ttu-id="6d81a-141">[![JavaScript 資料隱碼](preventing-javascript-injection-attacks-cs/_static/image5.png)](preventing-javascript-injection-attacks-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="6d81a-141">[![JavaScript Injection](preventing-javascript-injection-attacks-cs/_static/image5.png)](preventing-javascript-injection-attacks-cs/_static/image4.png)</span></span>

<span data-ttu-id="6d81a-142">**圖 02**: JavaScript 插入 ([按一下以檢視完整大小的影像](preventing-javascript-injection-attacks-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="6d81a-142">**Figure 02**: JavaScript Injection ([Click to view full-size image](preventing-javascript-injection-attacks-cs/_static/image6.png))</span></span>


<span data-ttu-id="6d81a-143">現在，您的初始回應 JavaScript 資料隱碼攻擊可能種冷漠態度。</span><span class="sxs-lookup"><span data-stu-id="6d81a-143">Now, your initial response to JavaScript injection attacks might be apathy.</span></span> <span data-ttu-id="6d81a-144">您可以將 JavaScript 資料隱碼攻擊是只是一種*損毀*攻擊。</span><span class="sxs-lookup"><span data-stu-id="6d81a-144">You might think that JavaScript injection attacks are simply a type of *defacement* attack.</span></span> <span data-ttu-id="6d81a-145">您可能認為，沒有人可以任何動作真正惡意所認可的 JavaScript 插入式攻擊。</span><span class="sxs-lookup"><span data-stu-id="6d81a-145">You might believe that no one can do anything truly evil by committing a JavaScript injection attack.</span></span>

<span data-ttu-id="6d81a-146">不幸的是，駭客就可以進行一些 really 真的惡意的項目插入至網站的 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="6d81a-146">Unfortunately, a hacker can do some really, really evil things by injecting JavaScript into a website.</span></span> <span data-ttu-id="6d81a-147">您可以使用 JavaScript 插入式攻擊，以執行跨網站指令碼 (XSS) 攻擊。</span><span class="sxs-lookup"><span data-stu-id="6d81a-147">You can use a JavaScript injection attack to perform a Cross-Site Scripting (XSS) attack.</span></span> <span data-ttu-id="6d81a-148">在跨網站指令碼處理攻擊中，您會竊取機密的使用者資訊，並將資訊傳送給另一個網站。</span><span class="sxs-lookup"><span data-stu-id="6d81a-148">In a Cross-Site Scripting attack, you steal confidential user information and send the information to another website.</span></span>

<span data-ttu-id="6d81a-149">比方說，駭客就可以使用 JavaScript 資料隱碼攻擊竊取其他使用者的瀏覽器 cookie 的值。</span><span class="sxs-lookup"><span data-stu-id="6d81a-149">For example, a hacker can use a JavaScript injection attack to steal the values of browser cookies from other users.</span></span> <span data-ttu-id="6d81a-150">如果瀏覽器 cookie 中儲存機密資訊-例如密碼、 信用卡號碼或身分證號碼 –，駭客可以竊取這項資訊使用 JavaScript 插入式攻擊。</span><span class="sxs-lookup"><span data-stu-id="6d81a-150">If sensitive information -- such as passwords, credit card numbers, or social security numbers – is stored in the browser cookies, then a hacker can use a JavaScript injection attack to steal this information.</span></span> <span data-ttu-id="6d81a-151">或者，如果使用者在駭客就可以使用插入的 JavaScript 來抓取表單資料，並將它傳送給另一個網站已遭洩漏 JavaScript 攻擊中，在頁面所包含的表單欄位中輸入機密資訊。</span><span class="sxs-lookup"><span data-stu-id="6d81a-151">Or, if a user enters sensitive information in a form field contained in a page that has been compromised with a JavaScript attack, then the hacker can use the injected JavaScript to grab the form data and send it to another website.</span></span>

<span data-ttu-id="6d81a-152">*請害怕*。</span><span class="sxs-lookup"><span data-stu-id="6d81a-152">*Please be scared*.</span></span> <span data-ttu-id="6d81a-153">重視 JavaScript 插入式攻擊，並保護您的使用者機密資訊。</span><span class="sxs-lookup"><span data-stu-id="6d81a-153">Take JavaScript injection attacks seriously and protect your user's confidential information.</span></span> <span data-ttu-id="6d81a-154">在下面兩個區段中，我們將討論兩項技術可讓您保護您的 ASP.NET MVC 應用程式，從 JavaScript 資料隱碼攻擊。</span><span class="sxs-lookup"><span data-stu-id="6d81a-154">In the next two sections, we discuss two techniques that you can use to defend your ASP.NET MVC applications from JavaScript injection attacks.</span></span>

## <a name="approach-1-html-encode-in-the-view"></a><span data-ttu-id="6d81a-155">方法 1： 在檢視中的 HTML 編碼</span><span class="sxs-lookup"><span data-stu-id="6d81a-155">Approach #1: HTML Encode in the View</span></span>

<span data-ttu-id="6d81a-156">一個簡單的方法，防止 JavaScript 插入式攻擊是 HTML 編碼重新顯示在檢視中的資料時，網站使用者所輸入的任何資料。</span><span class="sxs-lookup"><span data-stu-id="6d81a-156">One easy method of preventing JavaScript injection attacks is to HTML encode any data entered by website users when you redisplay the data in a view.</span></span> <span data-ttu-id="6d81a-157">已更新`Index` 檢視中列出的 3 遵循此方法。</span><span class="sxs-lookup"><span data-stu-id="6d81a-157">The updated `Index` view in Listing 3 follows this approach.</span></span>

<span data-ttu-id="6d81a-158">**列出 3 – `Index.aspx` (HTML 編碼)**</span><span class="sxs-lookup"><span data-stu-id="6d81a-158">**Listing 3 – `Index.aspx` (HTML Encoded)**</span></span>

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample4.aspx)]

<span data-ttu-id="6d81a-159">請注意，值`feedback.Message`是 HTML 編碼的值顯示為下列程式碼之前：</span><span class="sxs-lookup"><span data-stu-id="6d81a-159">Notice that the value of `feedback.Message` is HTML encoded before the value is displayed with the following code:</span></span>

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample5.aspx)]

<span data-ttu-id="6d81a-160">什麼平均值為 HTML 編碼字串？</span><span class="sxs-lookup"><span data-stu-id="6d81a-160">What does it mean to HTML encode a string?</span></span> <span data-ttu-id="6d81a-161">當您以 HTML 編碼字串時，請危險字元，例如`<`和`>`HTML 實體參考，例如以取代`&lt;`和`&gt;`。</span><span class="sxs-lookup"><span data-stu-id="6d81a-161">When you HTML encode a string, dangerous characters such as `<` and `>` are replaced by HTML entity references such as `&lt;` and `&gt;`.</span></span> <span data-ttu-id="6d81a-162">因此字串`<script>alert("Boo!")</script>`是 HTML 編碼，則取得轉換成`&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`。</span><span class="sxs-lookup"><span data-stu-id="6d81a-162">So when the string `<script>alert("Boo!")</script>` is HTML encoded, it gets converted to `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`.</span></span> <span data-ttu-id="6d81a-163">會由瀏覽器的解譯時，JavaScript 指令碼不會再執行編碼的字串。</span><span class="sxs-lookup"><span data-stu-id="6d81a-163">The encoded string no longer executes as a JavaScript script when interpreted by a browser.</span></span> <span data-ttu-id="6d81a-164">相反地，您會收到無害頁面圖 3 中。</span><span class="sxs-lookup"><span data-stu-id="6d81a-164">Instead, you get the harmless page in Figure 3.</span></span>


<span data-ttu-id="6d81a-165">[![讓的 JavaScript 攻擊](preventing-javascript-injection-attacks-cs/_static/image8.png)](preventing-javascript-injection-attacks-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="6d81a-165">[![Defeated JavaScript Attack](preventing-javascript-injection-attacks-cs/_static/image8.png)](preventing-javascript-injection-attacks-cs/_static/image7.png)</span></span>

<span data-ttu-id="6d81a-166">**圖 03**： 擊敗 JavaScript 攻擊 ([按一下以檢視完整大小的影像](preventing-javascript-injection-attacks-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="6d81a-166">**Figure 03**: Defeated JavaScript Attack ([Click to view full-size image](preventing-javascript-injection-attacks-cs/_static/image9.png))</span></span>


<span data-ttu-id="6d81a-167">請注意，在`Index`檢視中列出的 3 的值`feedback.Message`編碼。</span><span class="sxs-lookup"><span data-stu-id="6d81a-167">Notice that in the `Index` view in Listing 3 only the value of `feedback.Message` is encoded.</span></span> <span data-ttu-id="6d81a-168">值`feedback.EntryDate`就未編碼。</span><span class="sxs-lookup"><span data-stu-id="6d81a-168">The value of `feedback.EntryDate` is not encoded.</span></span> <span data-ttu-id="6d81a-169">您只需要使用者輸入的資料進行編碼。</span><span class="sxs-lookup"><span data-stu-id="6d81a-169">You only need to encode data entered by a user.</span></span> <span data-ttu-id="6d81a-170">由於控制器已產生 EntryDate 的值，因此您不需要 html 編碼這個值。</span><span class="sxs-lookup"><span data-stu-id="6d81a-170">Because the value of EntryDate was generated in the controller, you don't need to HTML encode this value.</span></span>

## <a name="approach-2-html-encode-in-the-controller"></a><span data-ttu-id="6d81a-171">方法 2： 在控制器中的 HTML 編碼</span><span class="sxs-lookup"><span data-stu-id="6d81a-171">Approach #2: HTML Encode in the Controller</span></span>

<span data-ttu-id="6d81a-172">而不是 HTML 編碼的資料，當您在檢視中顯示資料時，您可以 HTML 編碼的資料之前您送出至資料庫資料。</span><span class="sxs-lookup"><span data-stu-id="6d81a-172">Instead of HTML encoding data when you display the data in a view, you can HTML encode the data just before you submit the data to the database.</span></span> <span data-ttu-id="6d81a-173">會從這個第二種方法是`controller`中列出的 4。</span><span class="sxs-lookup"><span data-stu-id="6d81a-173">This second approach is taken in the case of the `controller` in Listing 4.</span></span>

<span data-ttu-id="6d81a-174">**列出 4 – `HomeController.cs` (HTML 編碼)**</span><span class="sxs-lookup"><span data-stu-id="6d81a-174">**Listing 4 – `HomeController.cs` (HTML Encoded)**</span></span>

[!code-csharp[Main](preventing-javascript-injection-attacks-cs/samples/sample6.cs)]

<span data-ttu-id="6d81a-175">請注意，訊息的值是 HTML 編碼的值會送出到資料庫中之前`Create()`動作。</span><span class="sxs-lookup"><span data-stu-id="6d81a-175">Notice that the value of Message is HTML encoded before the value is submitted to the database within the `Create()` action.</span></span> <span data-ttu-id="6d81a-176">當訊息會重新顯示在檢視中時，訊息是 HTML 編碼，而且不會執行任何插入訊息中的 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="6d81a-176">When the Message is redisplayed in the view, the Message is HTML encoded and any JavaScript injected in the Message is not executed.</span></span>

<span data-ttu-id="6d81a-177">一般而言，您可能偏好在本教學課程所討論，透過這個第二種方法的第一個方法。</span><span class="sxs-lookup"><span data-stu-id="6d81a-177">Typically, you should favor the first approach discussed in this tutorial over this second approach.</span></span> <span data-ttu-id="6d81a-178">此第二個方法的問題是，您以結束 HTML 編碼的資料在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="6d81a-178">The problem with this second approach is that you end up with HTML encoded data in your database.</span></span> <span data-ttu-id="6d81a-179">換句話說，不論您資料庫的資料是與有趣的字元。</span><span class="sxs-lookup"><span data-stu-id="6d81a-179">In other words, your database data is dirtied with funny looking characters.</span></span>

<span data-ttu-id="6d81a-180">為什麼這是不正確？</span><span class="sxs-lookup"><span data-stu-id="6d81a-180">Why is this bad?</span></span> <span data-ttu-id="6d81a-181">如果您需要在網頁以外的項目中顯示的資料庫資料，您就會發生問題。</span><span class="sxs-lookup"><span data-stu-id="6d81a-181">If you ever need to display the database data in something other than a web page, then you will have problems.</span></span> <span data-ttu-id="6d81a-182">比方說，您可以在 Windows Form 應用程式不再輕鬆地顯示資料。</span><span class="sxs-lookup"><span data-stu-id="6d81a-182">For example, you can no longer easily display the data in a Windows Forms application.</span></span>

## <a name="summary"></a><span data-ttu-id="6d81a-183">總結</span><span class="sxs-lookup"><span data-stu-id="6d81a-183">Summary</span></span>

<span data-ttu-id="6d81a-184">本教學課程的用途是為了嚇到您的 JavaScript 插入式攻擊的潛在相關。</span><span class="sxs-lookup"><span data-stu-id="6d81a-184">The purpose of this tutorial was to scare you about the prospect of a JavaScript injection attack.</span></span> <span data-ttu-id="6d81a-185">本教學課程所討論的防禦 JavaScript 資料隱碼攻擊您的 ASP.NET MVC 應用程式的兩種方法： 可以是 HTML 編碼提交的使用者資料的檢視，或者您可以 HTML 編碼使用者提交控制器中的資料。</span><span class="sxs-lookup"><span data-stu-id="6d81a-185">This tutorial discussed two approaches for defending your ASP.NET MVC applications against JavaScript injection attacks: you can either HTML encode user submitted data in the view or you can HTML encode user submitted data in the controller.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="6d81a-186">[上一頁](authenticating-users-with-windows-authentication-cs.md)
[下一頁](authenticating-users-with-forms-authentication-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6d81a-186">[Previous](authenticating-users-with-windows-authentication-cs.md)
[Next](authenticating-users-with-forms-authentication-vb.md)</span></span>