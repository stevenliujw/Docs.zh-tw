---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
title: "設定生產環境 Web 應用程式使用實際執行資料庫 (C#) |Microsoft 文件"
author: rick-anderson
description: "如先前的教學課程所述，是很常見的開發和生產環境之間差異的組態資訊。 這是 es..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 0177dabd-d888-449f-91b2-24190cf5e842
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 21eac6a4d829795f02eeeca5f9870b1ab8132d08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="configuring-the-production-web-application-to-use-the-production-database-c"></a><span data-ttu-id="f25e4-104">設定生產環境 Web 應用程式使用實際執行資料庫 (C#)</span><span class="sxs-lookup"><span data-stu-id="f25e4-104">Configuring the Production Web Application to Use the Production Database (C#)</span></span>
====================
<span data-ttu-id="f25e4-105">由[Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="f25e4-105">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="f25e4-106">[下載程式碼](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_CS.zip)或[下載 PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_cs.pdf)</span><span class="sxs-lookup"><span data-stu-id="f25e4-106">[Download Code](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_CS.zip) or [Download PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_cs.pdf)</span></span>

> <span data-ttu-id="f25e4-107">如先前的教學課程所述，是很常見的開發和生產環境之間差異的組態資訊。</span><span class="sxs-lookup"><span data-stu-id="f25e4-107">As discussed in earlier tutorials, it is not uncommon for configuration information to differ between the development and production environments.</span></span> <span data-ttu-id="f25e4-108">因為開發和生產環境之間的資料庫連接字串不同，這是資料驅動的 web 應用程式特別有用。</span><span class="sxs-lookup"><span data-stu-id="f25e4-108">This is especially true for data-driven web applications, as the database connection strings differ between the development and production environments.</span></span> <span data-ttu-id="f25e4-109">本教學課程中，瀏覽方式來設定要在更多詳細資料中包含適當的連接字串的實際執行環境。</span><span class="sxs-lookup"><span data-stu-id="f25e4-109">This tutorial explores ways to configure the production environment to include the appropriate connection string in more detail.</span></span>


## <a name="introduction"></a><span data-ttu-id="f25e4-110">簡介</span><span class="sxs-lookup"><span data-stu-id="f25e4-110">Introduction</span></span>

<span data-ttu-id="f25e4-111">資料驅動的 web 應用程式通常會在生產環境中使用不同的資料庫，在比開發中。</span><span class="sxs-lookup"><span data-stu-id="f25e4-111">Data-driven web applications usually use a different database when in development than when in production.</span></span> <span data-ttu-id="f25e4-112">裝載的 web 主機提供者，然後在本機開發應用程式，開發資料庫一般存在於開發人員電腦上實際執行資料庫裝載在虛擬主機公司的設備的資料庫伺服器上時。</span><span class="sxs-lookup"><span data-stu-id="f25e4-112">For applications hosted by a web host provider and developed locally, the development database typically resides on the developer s computer while the production database is hosted on a database server at the web hosting company s facility.</span></span> <span data-ttu-id="f25e4-113">資料驅動的 web 應用程式部署需要將開發資料庫複製到實際執行資料庫伺服器。</span><span class="sxs-lookup"><span data-stu-id="f25e4-113">Deploying a data-driven web application entails copying the development database to the production database server.</span></span> <span data-ttu-id="f25e4-114">在上一個教學課程中我們探討了方式可以完成此步驟。</span><span class="sxs-lookup"><span data-stu-id="f25e4-114">In the previous tutorial we looked at ways to accomplish this step.</span></span>

<span data-ttu-id="f25e4-115">Web 應用程式使用中的資訊*連接字串*建立與資料庫連線。</span><span class="sxs-lookup"><span data-stu-id="f25e4-115">The web application uses the information in a *connection string* to establish a connection with the database.</span></span> <span data-ttu-id="f25e4-116">連接字串中，通常會儲存在`Web.config`，指定資料庫伺服器名稱、 資料庫、 安全性內容，以及其他資訊的名稱。</span><span class="sxs-lookup"><span data-stu-id="f25e4-116">The connection string, which is typically stored in `Web.config`, specifies the database server name, the name of the database, the security context, and other information.</span></span> <span data-ttu-id="f25e4-117">Web 應用程式所使用的資料庫相依於是否正在開發或實際執行環境中執行 web 應用程式，因為連接字串必須與不同的兩種環境之間。</span><span class="sxs-lookup"><span data-stu-id="f25e4-117">Because the database used by the web application depends on whether the web application is running in the development or production environments, the connection strings must differ between the two environments.</span></span>

<span data-ttu-id="f25e4-118">是很常見的開發和生產環境之間差異的組態資訊的位置。</span><span class="sxs-lookup"><span data-stu-id="f25e4-118">It is not uncommon for configuration information to differ between the development and production environments.</span></span> <span data-ttu-id="f25e4-119">*通用組態差異之間開發和生產*教學課程所討論的技術上維護個別的組態資訊，這兩種環境，以及簡短討論之間資料庫連接字串。</span><span class="sxs-lookup"><span data-stu-id="f25e4-119">The *Common Configuration Differences Between Development and Production* tutorial discussed techniques for maintaining separate configuration information between these two environments, as well as a brief discussion on database connection strings.</span></span> <span data-ttu-id="f25e4-120">本教學課程中，瀏覽方式來設定要在更多詳細資料中包含適當的連接字串的實際執行環境。</span><span class="sxs-lookup"><span data-stu-id="f25e4-120">This tutorial explores ways to configure the production environment to include the appropriate connection string in more detail.</span></span>

## <a name="examining-the-connection-string-information"></a><span data-ttu-id="f25e4-121">檢查連接字串資訊</span><span class="sxs-lookup"><span data-stu-id="f25e4-121">Examining the Connection String Information</span></span>

<span data-ttu-id="f25e4-122">活頁簿檢閱 web 應用程式所使用的連接字串儲存在應用程式的組態檔， `Web.config`。</span><span class="sxs-lookup"><span data-stu-id="f25e4-122">The connection string used by the Book Reviews web application is stored in the application s configuration file, `Web.config`.</span></span> <span data-ttu-id="f25e4-123">`Web.config`包含儲存連接字串，命名相似的特殊區段[ &lt;connectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx)。</span><span class="sxs-lookup"><span data-stu-id="f25e4-123">`Web.config` includes a special section for storing connection strings, aptly named [&lt;connectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx).</span></span> <span data-ttu-id="f25e4-124">`Web.config`檔案書籍檢閱網站有一個名為此區段中定義的連接字串`ReviewsConnectionString`:</span><span class="sxs-lookup"><span data-stu-id="f25e4-124">The `Web.config` file for the Book Reviews website has one connection string defined in this section named `ReviewsConnectionString`:</span></span>

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample1.xml)]

<span data-ttu-id="f25e4-125">連接字串的資料來源 =。 \SQLEXPRESS。AttachDbFilename = |DataDirectory|\Reviews.mdf;Integrated 安全性 = True;使用者執行個體 = True-組成選項和值的數目，與選項/值組，以分號和每個選項分隔和等號分隔的值。</span><span class="sxs-lookup"><span data-stu-id="f25e4-125">The connection string - Data Source=.\SQLEXPRESS;AttachDbFilename=|DataDirectory|\Reviews.mdf;Integrated Security=True;User Instance=True - is composed of a number of options and values, with option/value pairs delimited by a semicolon and each option and value delimited by an equals sign.</span></span> <span data-ttu-id="f25e4-126">此連接字串中使用的四個選項如下：</span><span class="sxs-lookup"><span data-stu-id="f25e4-126">The four options used in this connection string are:</span></span>

- <span data-ttu-id="f25e4-127">`Data Source`-指定資料庫伺服器和資料庫伺服器執行個體名稱的位置 （如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="f25e4-127">`Data Source` - specifies the location of the database server and the database server instance name (if any).</span></span> <span data-ttu-id="f25e4-128">值， `.\SQLEXPRESS`，就是資料庫伺服器和執行個體名稱。</span><span class="sxs-lookup"><span data-stu-id="f25e4-128">The value, `.\SQLEXPRESS`, is an example where there is a database server and an instance name.</span></span> <span data-ttu-id="f25e4-129">期間指定的資料庫伺服器位於與此應用程式; 相同的電腦執行個體名稱是`SQLEXPRESS`。</span><span class="sxs-lookup"><span data-stu-id="f25e4-129">The period specifies that the database server is on the same computer as the application; the instance name is `SQLEXPRESS`.</span></span>
- <span data-ttu-id="f25e4-130">`AttachDbFilename`-指定資料庫檔案的位置。</span><span class="sxs-lookup"><span data-stu-id="f25e4-130">`AttachDbFilename` - specifies the location of the database file.</span></span> <span data-ttu-id="f25e4-131">值包含預留位置`|DataDirectory|`，這是解析為 s 的應用程式的完整路徑`App_Data`在執行階段 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f25e4-131">The value contains the placeholder `|DataDirectory|`, which is resolved to the full path of the application s `App_Data` folder at runtime.</span></span>
- <span data-ttu-id="f25e4-132">`Integrated Security`-布林值，指出是否要連接到資料庫 (false) 或目前 Windows 帳戶認證 (true) 時，使用指定的使用者名稱/密碼。</span><span class="sxs-lookup"><span data-stu-id="f25e4-132">`Integrated Security` - a Boolean value that indicates whether to use a specified username/password when connecting to the database (false) or the current Windows account credentials (true).</span></span>
- <span data-ttu-id="f25e4-133">`User Instance`-表示是否允許本機電腦上的非系統管理使用者連接和連接到 SQL Server Express Edition 資料庫的 SQL Server Express Edition 的特定組態選項。</span><span class="sxs-lookup"><span data-stu-id="f25e4-133">`User Instance` - a configuration option specific to the SQL Server Express Editions that indicates whether to allow non-Administrative users on the local computer attach and connect to a SQL Server Express Edition database.</span></span> <span data-ttu-id="f25e4-134">請參閱[SQL Server Express 使用者執行個體](https://msdn.microsoft.com/library/ms254504.aspx)如需有關這項設定。</span><span class="sxs-lookup"><span data-stu-id="f25e4-134">See [SQL Server Express User Instances](https://msdn.microsoft.com/library/ms254504.aspx) for more information on this setting.</span></span>
  

<span data-ttu-id="f25e4-135">允許的連接字串選項取決於您要連接到資料庫以及所使用的 ADO.NET 資料庫提供者。</span><span class="sxs-lookup"><span data-stu-id="f25e4-135">The allowable connection string options depend on the database you are connecting to and the ADO.NET database provider being used.</span></span> <span data-ttu-id="f25e4-136">例如，連接字串連接到 Microsoft SQL Server 資料庫與不同，用來連接到 Oracle 資料庫。</span><span class="sxs-lookup"><span data-stu-id="f25e4-136">For example, the connection string for connecting to a Microsoft SQL Server database differs from that used to connect to an Oracle database.</span></span> <span data-ttu-id="f25e4-137">同樣地，連接到 Microsoft SQL Server 資料庫使用 SqlClient 提供者會使用不同的連接字串比時使用的 OLE DB 提供者。</span><span class="sxs-lookup"><span data-stu-id="f25e4-137">Likewise, connecting to a Microsoft SQL Server database using the SqlClient provider uses a different connection string than when using the OLE-DB provider.</span></span>

<span data-ttu-id="f25e4-138">您可以使用像是站台手動建立資料庫連接字串[ConnectionStrings.com](http://www.connectionstrings.com/)為有哪些選項的資源。</span><span class="sxs-lookup"><span data-stu-id="f25e4-138">You can build the database connection string by hand using a site like [ConnectionStrings.com](http://www.connectionstrings.com/) as a resource for what options are available.</span></span> <span data-ttu-id="f25e4-139">不過，比較簡單的方式是將資料庫加入至 Visual Studio 中的 [伺服器總管]，然後抓取 [屬性] 視窗的連接字串。</span><span class="sxs-lookup"><span data-stu-id="f25e4-139">However, an easier approach is to add the database to the Server Explorer in Visual Studio and then to grab the connection string from the Properties window.</span></span> <span data-ttu-id="f25e4-140">可讓 s 建構生產資料庫伺服器的連接字串使用後者這種技術。</span><span class="sxs-lookup"><span data-stu-id="f25e4-140">Let s use this latter technique for constructing the connection string to the production database server.</span></span>

<span data-ttu-id="f25e4-141">開啟 Visual Studio，然後瀏覽至 伺服器總管 視窗 （在 Visual Web Developer 中，此視窗稱為資料庫總管）。</span><span class="sxs-lookup"><span data-stu-id="f25e4-141">Open Visual Studio and then navigate to the Server Explorer window (in Visual Web Developer, this window is called Database Explorer).</span></span> <span data-ttu-id="f25e4-142">以滑鼠右鍵按一下 [資料連線] 選項，並從內容功能表選擇 [加入連接] 選項。</span><span class="sxs-lookup"><span data-stu-id="f25e4-142">Right-click on the Data Connections option and choose the Add Connection option from the context menu.</span></span> <span data-ttu-id="f25e4-143">這會顯示在圖 1 所示的精靈。</span><span class="sxs-lookup"><span data-stu-id="f25e4-143">This brings up the wizard shown in Figure 1.</span></span> <span data-ttu-id="f25e4-144">選擇適當的資料來源，然後按一下 [繼續]。</span><span class="sxs-lookup"><span data-stu-id="f25e4-144">Choose the appropriate data source and click Continue.</span></span>


<span data-ttu-id="f25e4-145">[![選擇將新的資料庫加入至伺服器總管](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image1.jpg)</span><span class="sxs-lookup"><span data-stu-id="f25e4-145">[![Choose to Add a New Database to the Server Explorer](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image1.jpg)</span></span> 

<span data-ttu-id="f25e4-146">**圖 1**： 選擇將新的資料庫加入至 [伺服器總管] ([按一下以檢視完整大小的影像](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image3.jpg))</span><span class="sxs-lookup"><span data-stu-id="f25e4-146">**Figure 1**: Choose to Add a New Database to the Server Explorer ([Click to view full-size image](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image3.jpg))</span></span>


<span data-ttu-id="f25e4-147">接下來，指定各種資料庫連接資訊 （請參閱圖 2）。</span><span class="sxs-lookup"><span data-stu-id="f25e4-147">Next, specify the various database connection information (see Figure 2).</span></span> <span data-ttu-id="f25e4-148">當您註冊您的 web 與控管的公司他們應該如何提供資訊連接到資料庫-資料庫伺服器名稱、 資料庫名稱、 使用者名稱和密碼，用來連接到資料庫，並以此類推。</span><span class="sxs-lookup"><span data-stu-id="f25e4-148">When you signed up with your web hosting company they should have provided information on how to connect to the database - the database server name, the database name, the username and password to use to connect to the database, and so on.</span></span> <span data-ttu-id="f25e4-149">輸入這項資訊之後按一下 [確定] 完成這個精靈，並將資料庫加入至 [伺服器總管] 中。</span><span class="sxs-lookup"><span data-stu-id="f25e4-149">After entering this information click OK to complete this wizard and to add the database to the Server Explorer.</span></span>


<span data-ttu-id="f25e4-150">[![指定資料庫連接資訊](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image4.jpg)</span><span class="sxs-lookup"><span data-stu-id="f25e4-150">[![Specify the Database Connection Information](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image4.jpg)</span></span> 

<span data-ttu-id="f25e4-151">**圖 2**： 指定資料庫連接資訊 ([按一下以檢視完整大小的影像](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image6.jpg))</span><span class="sxs-lookup"><span data-stu-id="f25e4-151">**Figure 2**: Specify the Database Connection Information ([Click to view full-size image](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image6.jpg))</span></span>


<span data-ttu-id="f25e4-152">生產環境資料庫現在應該會顯示在 [伺服器總管] 中。</span><span class="sxs-lookup"><span data-stu-id="f25e4-152">The production environment database should now be listed in the Server Explorer.</span></span> <span data-ttu-id="f25e4-153">從 [伺服器總管] 中選取資料庫，並移至 [屬性] 視窗。</span><span class="sxs-lookup"><span data-stu-id="f25e4-153">Select the database from the Server Explorer and go to the Properties window.</span></span> <span data-ttu-id="f25e4-154">您將找到名為資料庫的連接字串與連接字串的屬性。</span><span class="sxs-lookup"><span data-stu-id="f25e4-154">There you will find a property named Connection String with the database s connection string.</span></span> <span data-ttu-id="f25e4-155">假設您在生產環境和 SqlClient 提供者上使用 Microsoft SQL Server 資料庫連接字串看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="f25e4-155">Assuming you are using a Microsoft SQL Server database on production and the SqlClient provider your connection string should look similar to the following:</span></span>

<span data-ttu-id="f25e4-156">**資料來源 =*serverName*;初始目錄 =*databaseName*;保存安全性資訊 = True;使用者 ID =*username*;密碼 = * 密碼***</span><span class="sxs-lookup"><span data-stu-id="f25e4-156">**Data Source=*serverName*; Initial Catalog=*databaseName*; Persist Security Info=True; User ID=*username*; Password=*password***</span></span>

<span data-ttu-id="f25e4-157">其中*serverName*， *databaseName*， *username*，和*密碼*與資料庫伺服器名稱，資料庫的值名稱，以及使用者名稱和密碼提供給您的 web 主機公司。</span><span class="sxs-lookup"><span data-stu-id="f25e4-157">Where *serverName*, *databaseName*, *username*, and *password* are with the values for the database server name, the database name, and the username and password supplied to you by your web host company.</span></span>

## <a name="deploying-the-book-reviews-web-application"></a><span data-ttu-id="f25e4-158">活頁簿檢閱 Web 應用程式部署</span><span class="sxs-lookup"><span data-stu-id="f25e4-158">Deploying the Book Reviews Web Application</span></span>

<span data-ttu-id="f25e4-159">前述教學課程會逐步執行將開發資料庫複製到生產環境，但沒有未瀏覽資料導向應用程式部署。</span><span class="sxs-lookup"><span data-stu-id="f25e4-159">The preceding tutorial stepped through copying the development database to the production environment, but did not explore deploying the data-driven application.</span></span> <span data-ttu-id="f25e4-160">此時在實際執行環境包含的資料庫，但使用靜態檢閱書籍檢閱應用程式的版本。</span><span class="sxs-lookup"><span data-stu-id="f25e4-160">At this point the production environment contains the database but is using the version of the Book Reviews application with static reviews.</span></span> <span data-ttu-id="f25e4-161">我們需要新的資料驅動應用程式部署至實際執行伺服器，以及更新的組態資訊。</span><span class="sxs-lookup"><span data-stu-id="f25e4-161">We need to deploy the new, data-driven application to the production server along with the updated configuration information.</span></span>

<span data-ttu-id="f25e4-162">請花一點時間部署資料導向應用程式從開發環境至實際執行環境。</span><span class="sxs-lookup"><span data-stu-id="f25e4-162">Take a moment to deploy the data-driven application from the development environment to production.</span></span> <span data-ttu-id="f25e4-163">在先前的教學課程中，此程序所涵蓋的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="f25e4-163">This process was covered in detail in previous tutorials.</span></span> <span data-ttu-id="f25e4-164">如果您需要重新整理程式，請參閱*部署您使用 FTP 用戶端的網站*或*部署您的網站使用 Visual Studio*教學課程。</span><span class="sxs-lookup"><span data-stu-id="f25e4-164">If you need a refresher, see the *Deploying Your Website Using an FTP Client* or the *Deploying Your Website Using Visual Studio* tutorials.</span></span> <span data-ttu-id="f25e4-165">您必須確保生產資料庫的連接字串是用在實際執行環境中，這表示替代`Web.config`檔案必須部署。</span><span class="sxs-lookup"><span data-stu-id="f25e4-165">You will need to ensure that the production database connection string is the one used in the production environment, which means that an alternate `Web.config` file must be deployed.</span></span> <span data-ttu-id="f25e4-166">具體而言，修改此設定`Web.config`檔案的`<connectionStrings>`項目必須包含的實際執行資料庫連接字串，而且看起來應該類似下列：</span><span class="sxs-lookup"><span data-stu-id="f25e4-166">Specifically, this modified `Web.config` file s `<connectionStrings>` element needs to contain the production database connection string and should look similar to the following:</span></span>

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample2.xml)]

<span data-ttu-id="f25e4-167">請注意，連接字串中`<connectionStrings>`元素的名稱相同 (`ReviewsConnectionString`)，但現在也包含生產資料庫的連接字串，而不是開發資料庫連接字串。</span><span class="sxs-lookup"><span data-stu-id="f25e4-167">Note that the connection string in the `<connectionStrings>` element is named the same (`ReviewsConnectionString`), but now contains the production database connection string instead of the development database connection string.</span></span>

<span data-ttu-id="f25e4-168">除非您有更正式的部署工作流程，請以手動方式修改`Web.config`部署 （記住還原成使用開發資料庫連接字串之前使用的實際執行資料庫連接字串的檔案之後） 或維護個別`Web.config`取得過程中上傳到生產環境的部署程序的實際執行環境組態資訊的檔案。</span><span class="sxs-lookup"><span data-stu-id="f25e4-168">Unless you have a more formalized deployment workflow, either manually modify the `Web.config` file to use the production database connection string before deploying (remembering to revert it back to using the development database connection string afterwards) or maintain a separate `Web.config` file with the production environment configuration information that gets uploaded to the production environment as part of the deployment process.</span></span>

> [!NOTE]
> <span data-ttu-id="f25e4-169">如果您不小心將部署`Web.config`包含開發資料庫連接字串，則會有錯誤時在實際執行應用程式嘗試連接到資料庫的檔案。</span><span class="sxs-lookup"><span data-stu-id="f25e4-169">If you accidentally deploy a `Web.config` file that contains the development database connection string then there will be an error when the application on production attempts to connect to the database.</span></span> <span data-ttu-id="f25e4-170">這個錯誤，卻使用`SqlException`與報告伺服器找不到或無法存取的訊息。</span><span class="sxs-lookup"><span data-stu-id="f25e4-170">This error manifests as a `SqlException` with a message reporting that the server was not found or was not accessible.</span></span>


<span data-ttu-id="f25e4-171">一旦部署網站至生產環境，請瀏覽整個瀏覽器將生產站台。</span><span class="sxs-lookup"><span data-stu-id="f25e4-171">Once the site has been deployed to production, visit the production site through your browser.</span></span> <span data-ttu-id="f25e4-172">您應看到及在本機執行資料驅動應用程式時享受與相同的使用者體驗。</span><span class="sxs-lookup"><span data-stu-id="f25e4-172">You should see and enjoy the same user experience as when running the data-driven application locally.</span></span> <span data-ttu-id="f25e4-173">當然您造訪的網站上實際執行時的站台是由所提供實際執行資料庫伺服器，而瀏覽的網站，在開發環境中使用的資料庫開發工作。</span><span class="sxs-lookup"><span data-stu-id="f25e4-173">Of course when you visit the website on production the site is powered by the production database server, whereas visiting the website in the development environment uses the database in development.</span></span> <span data-ttu-id="f25e4-174">圖 3 顯示*教導您自己 ASP.NET 3.5 24 小時內*檢閱頁面上，從實際執行環境 （請注意在瀏覽器的網址列中的 URL） 中的網站。</span><span class="sxs-lookup"><span data-stu-id="f25e4-174">Figure 3 shows the *Teach Yourself ASP.NET 3.5 in 24 Hours* review page from the website in the production environment (note the URL in the browser s Address bar).</span></span>


<span data-ttu-id="f25e4-175">[![資料驅動應用程式是現在提供在實際執行 ！](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image7.jpg)</span><span class="sxs-lookup"><span data-stu-id="f25e4-175">[![The Data-Driven Application is Now Available On Production!](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image7.jpg)</span></span> 

<span data-ttu-id="f25e4-176">**圖 3**: The Data-Driven 應用程式是現在提供在實際執行 ！</span><span class="sxs-lookup"><span data-stu-id="f25e4-176">**Figure 3**: The Data-Driven Application is Now Available On Production!</span></span> <span data-ttu-id="f25e4-177">([按一下以檢視完整大小的影像](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image9.jpg))</span><span class="sxs-lookup"><span data-stu-id="f25e4-177">([Click to view full-size image](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image9.jpg))</span></span>


### <a name="storing-connection-strings-in-a-separate-configuration-file"></a><span data-ttu-id="f25e4-178">將連接字串儲存在不同的組態檔</span><span class="sxs-lookup"><span data-stu-id="f25e4-178">Storing Connection Strings in a Separate Configuration File</span></span>

<span data-ttu-id="f25e4-179">常用的技巧來維護個別的組態資訊的開發和生產環境上是同時擁有兩個版本`Web.config`： 一個用於開發環境，一個用於生產環境。</span><span class="sxs-lookup"><span data-stu-id="f25e4-179">A common technique for maintaining separate configuration information on the development and production environments is to have two versions of the `Web.config`: one for the development environment and one for production.</span></span> <span data-ttu-id="f25e4-180">在部署階段適當`Web.config`版本可以複製到實際執行環境。</span><span class="sxs-lookup"><span data-stu-id="f25e4-180">At deploy-time the appropriate `Web.config` version can be copied to the production environment.</span></span> <span data-ttu-id="f25e4-181">在理想情況下，此程序會自動部署工作流程的一部分。</span><span class="sxs-lookup"><span data-stu-id="f25e4-181">Ideally, this process would be automated as part of the deployment workflow.</span></span>

<span data-ttu-id="f25e4-182">而不是維護兩個不同`Web.config`檔案，您可以選擇性地提供更細微的差異。</span><span class="sxs-lookup"><span data-stu-id="f25e4-182">Instead of maintaining two separate `Web.config` files you can, optionally, provide more granular differences.</span></span> <span data-ttu-id="f25e4-183">構成項目`Web.config`然後中參考外部組態檔中可以定義檔案`Web.config`檔案。</span><span class="sxs-lookup"><span data-stu-id="f25e4-183">The elements that make up the `Web.config` file can be defined in an external configuration files that are then referenced in the `Web.config` file.</span></span> <span data-ttu-id="f25e4-184">簡單的說，您可以有一個`Web.config`檔案參考 databaseConnectionStrings.config 檔案，其中將包含連接字串的這兩種環境由應用程式，而且是唯一的每個環境。</span><span class="sxs-lookup"><span data-stu-id="f25e4-184">In a nutshell you can have one `Web.config` file for both environments that references a databaseConnectionStrings.config file, which would contain the connection strings used by the application and would be unique for each environment.</span></span> <span data-ttu-id="f25e4-185">我發現分隔到不同的檔案不同的組態資訊提供一個更有條理且更簡單`Web.config`檔案和更清楚地列出組態之間的差異開發和生產環境。</span><span class="sxs-lookup"><span data-stu-id="f25e4-185">I find that separating the differing configuration information into separate files provides a tidier and simpler `Web.config` file and more clearly outlines the configuration differences between the development and production environments.</span></span>

<span data-ttu-id="f25e4-186">若要使用這項技術，啟動名為 web 應用程式中建立新資料夾`ConfigSections`。</span><span class="sxs-lookup"><span data-stu-id="f25e4-186">To use this technique, start by creating a new folder in the web application named `ConfigSections`.</span></span> <span data-ttu-id="f25e4-187">接下來，將兩個檔案加入至名為 databaseConnectionStrings.dev.config databaseConnectionStrings.production.config 此新資料夾。接下來，複製`<connectionStrings>`項目從`Web.config`在 databaseConnectionStrings.dev.config 和 databaseConnectionStrings.production.config 檔案，然後修改中的連接字串databaseConnectionStrings.production.config 檔案，使它指定生產資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="f25e4-187">Next, add two files to this new folder named databaseConnectionStrings.dev.config and databaseConnectionStrings.production.config. Next, copy the `<connectionStrings>` element from `Web.config` into the databaseConnectionStrings.dev.config and databaseConnectionStrings.production.config files, and then modify the connection string in the databaseConnectionStrings.production.config file so that it specifies the production database connection string.</span></span> <span data-ttu-id="f25e4-188">例如，databaseConnectionStrings.dev.config 檔案應該包含只`<connectionStrings>`參考開發資料庫的連接字串的項目：</span><span class="sxs-lookup"><span data-stu-id="f25e4-188">For example, the databaseConnectionStrings.dev.config file should contain just the `<connectionStrings>` element with a connection string that references the development database:</span></span>

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample3.xml)]

<span data-ttu-id="f25e4-189">同樣地，databaseConnectionStrings.production.config 檔案應該只包含`<connectionStrings>`項目，但有一個具有實際執行資料庫連線字串。</span><span class="sxs-lookup"><span data-stu-id="f25e4-189">Similarly, the databaseConnectionStrings.production.config file should contain just a `<connectionStrings>` element, but one that has the production database connection string.</span></span>

<span data-ttu-id="f25e4-190">建立一份 databaseConnectionStrings.dev.config 檔案並將它命名為 databaseConnectionStrings.config。</span><span class="sxs-lookup"><span data-stu-id="f25e4-190">Make a copy of the databaseConnectionStrings.dev.config file and name it databaseConnectionStrings.config.</span></span>

> [!NOTE]
> <span data-ttu-id="f25e4-191">您可以命名組態檔以外的 databaseConnectionStrings.config，如您 d，例如`connectionStrings.config`或`dbInfo.config`。</span><span class="sxs-lookup"><span data-stu-id="f25e4-191">You can name the configuration file something other than databaseConnectionStrings.config, if you d like, such as `connectionStrings.config` or `dbInfo.config`.</span></span> <span data-ttu-id="f25e4-192">不過，請務必將檔案與`.config`延伸做為`.config`檔案，根據預設，不提供 ASP.NET 引擎。</span><span class="sxs-lookup"><span data-stu-id="f25e4-192">However, be sure to name the file with a `.config` extension as `.config` files are, by default, not served by the ASP.NET engine.</span></span> <span data-ttu-id="f25e4-193">如果您將檔案命名其他項目，如`connectionStrings.txt`，使用者指出其瀏覽器[www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt)並檢視檔案的內容 ！</span><span class="sxs-lookup"><span data-stu-id="f25e4-193">If you name the file something else, like `connectionStrings.txt`, a user could point their browser to [www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt) and view the contents of the file!</span></span>


<span data-ttu-id="f25e4-194">此時`ConfigSections`資料夾應包含三個檔案 （請參閱圖 4）。</span><span class="sxs-lookup"><span data-stu-id="f25e4-194">At this point the `ConfigSections` folder should contain three files (see Figure 4).</span></span> <span data-ttu-id="f25e4-195">DatabaseConnectionStrings.dev.config 和 databaseConnectionStrings.production.config 檔案分別包含開發和生產環境中，連接字串。</span><span class="sxs-lookup"><span data-stu-id="f25e4-195">The databaseConnectionStrings.dev.config and databaseConnectionStrings.production.config files contain the connection strings for the development and production environments, respectively.</span></span> <span data-ttu-id="f25e4-196">DatabaseConnectionStrings.config 檔案包含將 web 應用程式在執行階段所使用的連接字串資訊。</span><span class="sxs-lookup"><span data-stu-id="f25e4-196">The databaseConnectionStrings.config file contains the connection string information that will be used by the web application at runtime.</span></span> <span data-ttu-id="f25e4-197">因此，databaseConnectionStrings.config 檔案應該相同 databaseConnectionStrings.dev.config 檔案，在開發環境中，而實際執行 databaseConnectionStrings.config 檔案應該相同databaseConnectionStrings.production.config。</span><span class="sxs-lookup"><span data-stu-id="f25e4-197">Consequently, the databaseConnectionStrings.config file should be identical to the databaseConnectionStrings.dev.config file in the development environment, whereas on production the databaseConnectionStrings.config file should be identical to databaseConnectionStrings.production.config.</span></span>


<span data-ttu-id="f25e4-198">[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image10.jpg)</span><span class="sxs-lookup"><span data-stu-id="f25e4-198">[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image10.jpg)</span></span> 

<span data-ttu-id="f25e4-199">**圖 4**: C ([按一下以檢視完整大小的影像](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image12.jpg))</span><span class="sxs-lookup"><span data-stu-id="f25e4-199">**Figure 4**: ConfigSections ([Click to view full-size image](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image12.jpg))</span></span>


<span data-ttu-id="f25e4-200">我們現在需要指示`Web.config`databaseConnectionStrings.config 檔案用於其連接字串存放區。</span><span class="sxs-lookup"><span data-stu-id="f25e4-200">We now need to instruct `Web.config` to use the databaseConnectionStrings.config file for its connection string store.</span></span> <span data-ttu-id="f25e4-201">開啟`Web.config`並取代現有`<connectionStrings>`具有下列項目：</span><span class="sxs-lookup"><span data-stu-id="f25e4-201">Open `Web.config` and replace the existing `<connectionStrings>` element with the following:</span></span>

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample4.xml)]

<span data-ttu-id="f25e4-202">`configSource`屬性指定的實體路徑，相對於`Web.config`檔案。</span><span class="sxs-lookup"><span data-stu-id="f25e4-202">The `configSource` attribute specifies a physical path relative to the `Web.config` file.</span></span> <span data-ttu-id="f25e4-203">如果外部`.config`檔案位於相同的目錄`Web.config`然後將此屬性設定為的檔案名稱`.config`檔案。</span><span class="sxs-lookup"><span data-stu-id="f25e4-203">If the external `.config` file is in the same directory as `Web.config` then set this attribute to the file name of the `.config` file.</span></span> <span data-ttu-id="f25e4-204">如果它在子目錄中，使用 databaseConnectionStrings.config，案例指定使用反斜線分隔的資料夾和檔案名稱，例如 ConfigSections\databaseConnectionStrings.config 的子資料夾。</span><span class="sxs-lookup"><span data-stu-id="f25e4-204">If it s in a subdirectory, as is the case with databaseConnectionStrings.config, specify the subfolder using a backslash to delimit the folder and file names, like ConfigSections\databaseConnectionStrings.config.</span></span>

<span data-ttu-id="f25e4-205">這項修改，以開發和生產環境包含相同`Web.config`檔案。</span><span class="sxs-lookup"><span data-stu-id="f25e4-205">With this modification, the development and production environments contain the same `Web.config` file.</span></span> <span data-ttu-id="f25e4-206">現在，唯一的差別是 databaseConnectionStrings.config 檔案。</span><span class="sxs-lookup"><span data-stu-id="f25e4-206">Now the only difference is the databaseConnectionStrings.config file.</span></span> <span data-ttu-id="f25e4-207">DatabaseConnectionStrings.production.config 檔案複製到生產環境，並將它重新命名為 databaseConnectionStrings.config。如果日後有生產資料庫的連接字串變更您必須對 databaseConnectionStrings.production.config 檔案，然後將該檔案上傳到生產環境，databaseConnectionStrings.config 重新命名。</span><span class="sxs-lookup"><span data-stu-id="f25e4-207">Copy the databaseConnectionStrings.production.config file to production and rename it to databaseConnectionStrings.config. If, in the future, there are changes to the production database connection string you will need to make them to the databaseConnectionStrings.production.config file and then upload that file to production, renaming it databaseConnectionStrings.config.</span></span>

> [!NOTE]
> <span data-ttu-id="f25e4-208">您可以指定任何資訊`Web.config`個別的檔案和使用中的項目`configSource`屬性，來參照該檔案從`Web.config`。</span><span class="sxs-lookup"><span data-stu-id="f25e4-208">You may specify the information for any `Web.config` element in a separate file and use the `configSource` attribute to reference that file from within `Web.config`.</span></span>


## <a name="summary"></a><span data-ttu-id="f25e4-209">總結</span><span class="sxs-lookup"><span data-stu-id="f25e4-209">Summary</span></span>

<span data-ttu-id="f25e4-210">資料驅動應用程式通常會在開發和生產環境中使用不同的資料庫。</span><span class="sxs-lookup"><span data-stu-id="f25e4-210">Data-driven applications usually use different databases in the development and production environments.</span></span> <span data-ttu-id="f25e4-211">因此，儲存在 web 應用程式的組態中的資料庫連接字串必須是唯一每個環境。</span><span class="sxs-lookup"><span data-stu-id="f25e4-211">Consequently, the database connection strings stored in the web application s configuration must be unique per environment.</span></span> <span data-ttu-id="f25e4-212">在此教學課程中我們討論了如何判斷實際執行資料庫連接字串和維護兩個環境中的唯一連接字串資訊的方式。</span><span class="sxs-lookup"><span data-stu-id="f25e4-212">In this tutorial we looked at how to determine the production database connection string and ways to maintain unique connection string information in the two environments.</span></span>

<span data-ttu-id="f25e4-213">祝您程式設計 ！</span><span class="sxs-lookup"><span data-stu-id="f25e4-213">Happy Programming!</span></span>

#### <a name="further-reading"></a><span data-ttu-id="f25e4-214">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="f25e4-214">Further Reading</span></span>

<span data-ttu-id="f25e4-215">如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="f25e4-215">For more information on the topics discussed in this tutorial, refer to the following resources:</span></span>

- [<span data-ttu-id="f25e4-216">連接字串和組態檔</span><span class="sxs-lookup"><span data-stu-id="f25e4-216">Connection Strings and Configuration Files</span></span>](https://msdn.microsoft.com/library/ms254494.aspx)
- [<span data-ttu-id="f25e4-217">資料庫組態字串資訊 @ ConnectionStrings.com</span><span class="sxs-lookup"><span data-stu-id="f25e4-217">Database Configuration Strings Information @ ConnectionStrings.com</span></span>](http://www.connectionstrings.com/)
- [<span data-ttu-id="f25e4-218">設定移出 Web.config 檔案</span><span class="sxs-lookup"><span data-stu-id="f25e4-218">Move Settings Out of the Web.config File</span></span>](http://www.asp101.com/tips/index.asp?id=154)
- [<span data-ttu-id="f25e4-219">技術文件&lt;connectionStrings&gt;項目</span><span class="sxs-lookup"><span data-stu-id="f25e4-219">Technical Documentation for the &lt;connectionStrings&gt; Element</span></span>](https://msdn.microsoft.com/library/bf7sd233.aspx)

>[!div class="step-by-step"]
<span data-ttu-id="f25e4-220">[上一頁](deploying-a-database-cs.md)
[下一頁](configuring-a-website-that-uses-application-services-cs.md)</span><span class="sxs-lookup"><span data-stu-id="f25e4-220">[Previous](deploying-a-database-cs.md)
[Next](configuring-a-website-that-uses-application-services-cs.md)</span></span>