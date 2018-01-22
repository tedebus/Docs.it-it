---
title: Utilizzare i file statici in ASP.NET Core
author: rick-anderson
description: Informazioni su come gestire e proteggere i file statici e per configurare file statico che ospita i comportamenti di middleware in un'applicazione web ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: 60b205bf0a45e2965f9dab46f46956947ae513fd
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="work-with-static-files-in-aspnet-core"></a><span data-ttu-id="0ed37-103">Utilizzare i file statici in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0ed37-103">Work with static files in ASP.NET Core</span></span>

<span data-ttu-id="0ed37-104">Da [Rick Anderson](https://twitter.com/RickAndMSFT) e [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="0ed37-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="0ed37-105">File statici, ad esempio HTML, CSS, immagini e JavaScript, sono attività che viene utilizzata un'applicazione ASP.NET Core direttamente al client.</span><span class="sxs-lookup"><span data-stu-id="0ed37-105">Static files, such as HTML, CSS, images, and JavaScript, are assets an ASP.NET Core app serves directly to clients.</span></span> <span data-ttu-id="0ed37-106">Alcune attività di configurazione è necessario attivare la gestione di questi file.</span><span class="sxs-lookup"><span data-stu-id="0ed37-106">Some configuration is required to enable to serving of these files.</span></span>

<span data-ttu-id="0ed37-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0ed37-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="0ed37-108">Gestire file statici</span><span class="sxs-lookup"><span data-stu-id="0ed37-108">Serve static files</span></span>

<span data-ttu-id="0ed37-109">File statici vengono archiviati in directory di radice del progetto web.</span><span class="sxs-lookup"><span data-stu-id="0ed37-109">Static files are stored within your project's web root directory.</span></span> <span data-ttu-id="0ed37-110">La directory predefinita è  *\<content_root > / wwwroot*, ma può essere modificata tramite il [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) metodo.</span><span class="sxs-lookup"><span data-stu-id="0ed37-110">The default directory is *\<content_root>/wwwroot*, but it can be changed via the [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) method.</span></span> <span data-ttu-id="0ed37-111">Vedere [contenuto radice](xref:fundamentals/index#content-root) e [radice Web](xref:fundamentals/index#web-root) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="0ed37-111">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span>

<span data-ttu-id="0ed37-112">Dell'host dell'applicazione web deve essere a conoscenza della directory radice del contenuto.</span><span class="sxs-lookup"><span data-stu-id="0ed37-112">The app's web host must be made aware of the content root directory.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0ed37-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0ed37-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0ed37-114">Il `WebHost.CreateDefaultBuilder` metodo imposta la radice del contenuto nella directory corrente:</span><span class="sxs-lookup"><span data-stu-id="0ed37-114">The `WebHost.CreateDefaultBuilder` method sets the content root to the current directory:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0ed37-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0ed37-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0ed37-116">Imposta la radice del contenuto nella directory corrente richiamando [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) all'interno di `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="0ed37-116">Set the content root to the current directory by invoking [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) inside of `Program.Main`:</span></span>

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

<span data-ttu-id="0ed37-117">File statici sono accessibili tramite un percorso relativo alla radice web.</span><span class="sxs-lookup"><span data-stu-id="0ed37-117">Static files are accessible via a path relative to the web root.</span></span> <span data-ttu-id="0ed37-118">Ad esempio, il **applicazione Web** modello di progetto contiene diverse cartelle all'interno di *wwwroot* cartella:</span><span class="sxs-lookup"><span data-stu-id="0ed37-118">For example, the **Web Application** project template contains several folders within the *wwwroot* folder:</span></span>

* <span data-ttu-id="0ed37-119">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="0ed37-119">**wwwroot**</span></span>
  * <span data-ttu-id="0ed37-120">**css**</span><span class="sxs-lookup"><span data-stu-id="0ed37-120">**css**</span></span>
  * <span data-ttu-id="0ed37-121">**immagini**</span><span class="sxs-lookup"><span data-stu-id="0ed37-121">**images**</span></span>
  * <span data-ttu-id="0ed37-122">**js**</span><span class="sxs-lookup"><span data-stu-id="0ed37-122">**js**</span></span>

<span data-ttu-id="0ed37-123">Il formato dell'URI per accedere a un file di *immagini* sottocartella *http://\<server_address > /images/\<Nome_file_immagine >*.</span><span class="sxs-lookup"><span data-stu-id="0ed37-123">The URI format to access a file in the *images* subfolder is *http://\<server_address>/images/\<image_file_name>*.</span></span> <span data-ttu-id="0ed37-124">Ad esempio, *http://localhost:9189/images/banner3.svg*.</span><span class="sxs-lookup"><span data-stu-id="0ed37-124">For example, *http://localhost:9189/images/banner3.svg*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0ed37-125">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0ed37-125">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0ed37-126">Se la destinazione è .NET Framework, aggiungere il [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) pacchetto al progetto.</span><span class="sxs-lookup"><span data-stu-id="0ed37-126">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span> <span data-ttu-id="0ed37-127">Se la destinazione di .NET Core, il [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) include il pacchetto.</span><span class="sxs-lookup"><span data-stu-id="0ed37-127">If targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) includes this package.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0ed37-128">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0ed37-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0ed37-129">Aggiungere il [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) pacchetto al progetto.</span><span class="sxs-lookup"><span data-stu-id="0ed37-129">Add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span>

---

<span data-ttu-id="0ed37-130">Configurare il [middleware](xref:fundamentals/middleware) che consente la gestione dei file statici.</span><span class="sxs-lookup"><span data-stu-id="0ed37-130">Configure the [middleware](xref:fundamentals/middleware) which enables the serving of static files.</span></span>

### <a name="serve-files-inside-of-web-root"></a><span data-ttu-id="0ed37-131">File radice web all'interno di</span><span class="sxs-lookup"><span data-stu-id="0ed37-131">Serve files inside of web root</span></span>

<span data-ttu-id="0ed37-132">Richiamare il [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) metodo all'interno di `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="0ed37-132">Invoke the [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method within `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

<span data-ttu-id="0ed37-133">Senza parametri `UseStaticFiles` overload del metodo contrassegna i file nella radice web come servable.</span><span class="sxs-lookup"><span data-stu-id="0ed37-133">The parameterless `UseStaticFiles` method overload marks the files in web root as servable.</span></span> <span data-ttu-id="0ed37-134">I seguenti riferimenti markup *wwwroot/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="0ed37-134">The following markup references *wwwroot/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a><span data-ttu-id="0ed37-135">Gestire file esterni radice web</span><span class="sxs-lookup"><span data-stu-id="0ed37-135">Serve files outside of web root</span></span>

<span data-ttu-id="0ed37-136">Considerare una gerarchia di directory in cui si trovano i file statici servito all'esterno nella radice web:</span><span class="sxs-lookup"><span data-stu-id="0ed37-136">Consider a directory hierarchy in which the static files to be served reside outside of the web root:</span></span>

* <span data-ttu-id="0ed37-137">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="0ed37-137">**wwwroot**</span></span>
  * <span data-ttu-id="0ed37-138">**css**</span><span class="sxs-lookup"><span data-stu-id="0ed37-138">**css**</span></span>
  * <span data-ttu-id="0ed37-139">**immagini**</span><span class="sxs-lookup"><span data-stu-id="0ed37-139">**images**</span></span>
  * <span data-ttu-id="0ed37-140">**js**</span><span class="sxs-lookup"><span data-stu-id="0ed37-140">**js**</span></span>
* <span data-ttu-id="0ed37-141">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="0ed37-141">**MyStaticFiles**</span></span>
  * <span data-ttu-id="0ed37-142">**immagini**</span><span class="sxs-lookup"><span data-stu-id="0ed37-142">**images**</span></span>
      * <span data-ttu-id="0ed37-143">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="0ed37-143">*banner1.svg*</span></span>

<span data-ttu-id="0ed37-144">Accessibile a una richiesta di *banner1.svg* file configurando il middleware di file statici come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="0ed37-144">A request can access the *banner1.svg* file by configuring the static file middleware as follows:</span></span>

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

<span data-ttu-id="0ed37-145">Nel codice precedente, il *MyStaticFiles* gerarchia di directory viene esposto pubblicamente tramite la *StaticFiles* segmento URI.</span><span class="sxs-lookup"><span data-stu-id="0ed37-145">In the preceding code, the *MyStaticFiles* directory hierarchy is exposed publicly via the *StaticFiles* URI segment.</span></span> <span data-ttu-id="0ed37-146">Una richiesta di *http://\<server_address > /StaticFiles/images/banner1.svg* serve il *banner1.svg* file.</span><span class="sxs-lookup"><span data-stu-id="0ed37-146">A request to *http://\<server_address>/StaticFiles/images/banner1.svg* serves the *banner1.svg* file.</span></span>

<span data-ttu-id="0ed37-147">I seguenti riferimenti markup *MyStaticFiles/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="0ed37-147">The following markup references *MyStaticFiles/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a><span data-ttu-id="0ed37-148">Impostare le intestazioni di risposta HTTP</span><span class="sxs-lookup"><span data-stu-id="0ed37-148">Set HTTP response headers</span></span>

<span data-ttu-id="0ed37-149">Oggetto [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) oggetto può essere utilizzato per impostare le intestazioni di risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="0ed37-149">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) object can be used to set HTTP response headers.</span></span> <span data-ttu-id="0ed37-150">Oltre a configurare la gestione di file statici dalla radice web, il codice seguente imposta il `Cache-Control` intestazione:</span><span class="sxs-lookup"><span data-stu-id="0ed37-150">In addition to configuring static file serving from the web root, the following code sets the `Cache-Control` header:</span></span>

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="0ed37-151">Il [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) metodo è presente il [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="0ed37-151">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method exists in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

<span data-ttu-id="0ed37-152">I file sono stati apportati pubblicamente memorizzabile nella cache per 10 minuti (600 secondi):</span><span class="sxs-lookup"><span data-stu-id="0ed37-152">The files have been made publicly cacheable for 10 minutes (600 seconds):</span></span>

![Le intestazioni di risposta che mostra l'intestazione Cache-Control è stata aggiunta](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="0ed37-154">Autorizzazione del file statico</span><span class="sxs-lookup"><span data-stu-id="0ed37-154">Static file authorization</span></span>

<span data-ttu-id="0ed37-155">Il middleware di file statici non fornisce controlli di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="0ed37-155">The static file middleware doesn't provide authorization checks.</span></span> <span data-ttu-id="0ed37-156">Qualsiasi file forniti, tra cui quelle contenute *wwwroot*, sono accessibili pubblicamente.</span><span class="sxs-lookup"><span data-stu-id="0ed37-156">Any files served by it, including those under *wwwroot*, are publicly accessible.</span></span> <span data-ttu-id="0ed37-157">Per gestire i file in base alle autorizzazioni:</span><span class="sxs-lookup"><span data-stu-id="0ed37-157">To serve files based on authorization:</span></span>

* <span data-ttu-id="0ed37-158">Archiviarli all'esterno di *wwwroot* e qualsiasi directory accessibile per il middleware di file statici **e**</span><span class="sxs-lookup"><span data-stu-id="0ed37-158">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>
* <span data-ttu-id="0ed37-159">Esse forniscono tramite un metodo di azione a cui viene applicata l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="0ed37-159">Serve them via an action method to which authorization is applied.</span></span> <span data-ttu-id="0ed37-160">Restituire un [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) oggetto:</span><span class="sxs-lookup"><span data-stu-id="0ed37-160">Return a [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) object:</span></span>

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a><span data-ttu-id="0ed37-161">Abilitare l'esplorazione directory</span><span class="sxs-lookup"><span data-stu-id="0ed37-161">Enable directory browsing</span></span>

<span data-ttu-id="0ed37-162">Esplorazione directory consente agli utenti dell'app web visualizzare un elenco di directory e i file all'interno di una directory specificata.</span><span class="sxs-lookup"><span data-stu-id="0ed37-162">Directory browsing allows users of your web app to see a directory listing and files within a specified directory.</span></span> <span data-ttu-id="0ed37-163">Esplorazione directory è disabilitata per impostazione predefinita per motivi di sicurezza (vedere [considerazioni](#considerations)).</span><span class="sxs-lookup"><span data-stu-id="0ed37-163">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="0ed37-164">Directory Attivazione esplorazione richiamando il [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) metodo `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="0ed37-164">Enable directory browsing by invoking the [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) method in `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

<span data-ttu-id="0ed37-165">Aggiungere i servizi necessari richiamando il [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) metodo `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0ed37-165">Add required services by invoking the [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

<span data-ttu-id="0ed37-166">Il codice precedente consente l'esplorazione delle directory per il *wwwroot/immagini* cartella utilizzando l'URL *http://\<server_address > / MyImages*, con collegamenti ai singoli file e cartelle:</span><span class="sxs-lookup"><span data-stu-id="0ed37-166">The preceding code allows directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*, with links to each file and folder:</span></span>

![esplorazione directory](static-files/_static/dir-browse.png)

<span data-ttu-id="0ed37-168">Vedere [considerazioni](#considerations) sui rischi di sicurezza quando si abilita l'esplorazione.</span><span class="sxs-lookup"><span data-stu-id="0ed37-168">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="0ed37-169">Tenere presente le due `UseStaticFiles` chiama nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="0ed37-169">Note the two `UseStaticFiles` calls in the following example.</span></span> <span data-ttu-id="0ed37-170">La prima chiamata consente la gestione di file statici di *wwwroot* cartella.</span><span class="sxs-lookup"><span data-stu-id="0ed37-170">The first call enables the serving of static files in the *wwwroot* folder.</span></span> <span data-ttu-id="0ed37-171">La seconda chiamata consente l'esplorazione delle directory per il *wwwroot/immagini* cartella utilizzando l'URL *http://\<server_address > / MyImages*:</span><span class="sxs-lookup"><span data-stu-id="0ed37-171">The second call enables directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a><span data-ttu-id="0ed37-172">Serva un documento predefinito</span><span class="sxs-lookup"><span data-stu-id="0ed37-172">Serve a default document</span></span>

<span data-ttu-id="0ed37-173">L'impostazione di una home page predefinita fornisce visitatori un punto di partenza logico quando si visita il sito.</span><span class="sxs-lookup"><span data-stu-id="0ed37-173">Setting a default home page provides visitors a logical starting point when visiting your site.</span></span> <span data-ttu-id="0ed37-174">Per una pagina predefinita senza che l'utente in modo completo l'URI, chiamare il [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) metodo `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="0ed37-174">To serve a default page without the user fully qualifying the URI, call the [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method from `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> <span data-ttu-id="0ed37-175">`UseDefaultFiles`deve essere chiamato prima `UseStaticFiles` per servire il file predefinito.</span><span class="sxs-lookup"><span data-stu-id="0ed37-175">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="0ed37-176">`UseDefaultFiles`è un rewriter URL che non viene effettivamente servire il file.</span><span class="sxs-lookup"><span data-stu-id="0ed37-176">`UseDefaultFiles` is a URL rewriter that doesn't actually serve the file.</span></span> <span data-ttu-id="0ed37-177">Abilitare il middleware di file statici tramite `UseStaticFiles` per servire il file.</span><span class="sxs-lookup"><span data-stu-id="0ed37-177">Enable the static file middleware via `UseStaticFiles` to serve the file.</span></span>

<span data-ttu-id="0ed37-178">Con `UseDefaultFiles`, le richieste a una ricerca nella cartella per:</span><span class="sxs-lookup"><span data-stu-id="0ed37-178">With `UseDefaultFiles`, requests to a folder search for:</span></span>

* <span data-ttu-id="0ed37-179">*default.htm*</span><span class="sxs-lookup"><span data-stu-id="0ed37-179">*default.htm*</span></span>
* <span data-ttu-id="0ed37-180">*default.html*</span><span class="sxs-lookup"><span data-stu-id="0ed37-180">*default.html*</span></span>
* <span data-ttu-id="0ed37-181">*index.htm*</span><span class="sxs-lookup"><span data-stu-id="0ed37-181">*index.htm*</span></span>
* <span data-ttu-id="0ed37-182">*index.html*</span><span class="sxs-lookup"><span data-stu-id="0ed37-182">*index.html*</span></span>

<span data-ttu-id="0ed37-183">Il primo file trovato nell'elenco è disponibile come se la richiesta fosse l'URI completo.</span><span class="sxs-lookup"><span data-stu-id="0ed37-183">The first file found from the list is served as though the request were the fully qualified URI.</span></span> <span data-ttu-id="0ed37-184">URL del browser continua in modo da riflettere l'URI richiesto.</span><span class="sxs-lookup"><span data-stu-id="0ed37-184">The browser URL continues to reflect the URI requested.</span></span>

<span data-ttu-id="0ed37-185">Il codice seguente modifica il nome file predefinito per *mydefault.html*:</span><span class="sxs-lookup"><span data-stu-id="0ed37-185">The following code changes the default file name to *mydefault.html*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a><span data-ttu-id="0ed37-186">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="0ed37-186">UseFileServer</span></span>

<span data-ttu-id="0ed37-187">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combina la funzionalità di `UseStaticFiles`, `UseDefaultFiles`, e `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="0ed37-187">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="0ed37-188">Il codice seguente consente la gestione di file statici e il file predefinito.</span><span class="sxs-lookup"><span data-stu-id="0ed37-188">The following code enables the serving of static files and the default file.</span></span> <span data-ttu-id="0ed37-189">Non è abilitata l'esplorazione directory.</span><span class="sxs-lookup"><span data-stu-id="0ed37-189">Directory browsing isn't enabled.</span></span>

```csharp
app.UseFileServer();
```

<span data-ttu-id="0ed37-190">Il codice seguente si basa sull'overload senza parametri, consentendo l'esplorazione directory:</span><span class="sxs-lookup"><span data-stu-id="0ed37-190">The following code builds upon the parameterless overload by enabling directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

<span data-ttu-id="0ed37-191">Si consideri la seguente gerarchia di directory:</span><span class="sxs-lookup"><span data-stu-id="0ed37-191">Consider the following directory hierarchy:</span></span>

* <span data-ttu-id="0ed37-192">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="0ed37-192">**wwwroot**</span></span>
  * <span data-ttu-id="0ed37-193">**css**</span><span class="sxs-lookup"><span data-stu-id="0ed37-193">**css**</span></span>
  * <span data-ttu-id="0ed37-194">**immagini**</span><span class="sxs-lookup"><span data-stu-id="0ed37-194">**images**</span></span>
  * <span data-ttu-id="0ed37-195">**js**</span><span class="sxs-lookup"><span data-stu-id="0ed37-195">**js**</span></span>
* <span data-ttu-id="0ed37-196">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="0ed37-196">**MyStaticFiles**</span></span>
  * <span data-ttu-id="0ed37-197">**immagini**</span><span class="sxs-lookup"><span data-stu-id="0ed37-197">**images**</span></span>
      * <span data-ttu-id="0ed37-198">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="0ed37-198">*banner1.svg*</span></span>
  * <span data-ttu-id="0ed37-199">*default.html*</span><span class="sxs-lookup"><span data-stu-id="0ed37-199">*default.html*</span></span>

<span data-ttu-id="0ed37-200">Il codice seguente permette di file statici, i file predefiniti e esplorazione delle directory `MyStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="0ed37-200">The following code enables static files, default files, and directory browsing of `MyStaticFiles`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

<span data-ttu-id="0ed37-201">`AddDirectoryBrowser`deve essere chiamato quando il `EnableDirectoryBrowsing` valore della proprietà è `true`:</span><span class="sxs-lookup"><span data-stu-id="0ed37-201">`AddDirectoryBrowser` must be called when the `EnableDirectoryBrowsing` property value is `true`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

<span data-ttu-id="0ed37-202">URL di codice usando la gerarchia di file e precedenti, risolvere come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="0ed37-202">Using the file hierarchy and preceding code, URLs resolve as follows:</span></span>

| <span data-ttu-id="0ed37-203">URI</span><span class="sxs-lookup"><span data-stu-id="0ed37-203">URI</span></span>            |                             <span data-ttu-id="0ed37-204">Risposta</span><span class="sxs-lookup"><span data-stu-id="0ed37-204">Response</span></span>  |
| ------- | ------|
| <span data-ttu-id="0ed37-205">*http://\<server_address>/StaticFiles/images/banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="0ed37-205">*http://\<server_address>/StaticFiles/images/banner1.svg*</span></span>    |      <span data-ttu-id="0ed37-206">MyStaticFiles/images/banner1.svg</span><span class="sxs-lookup"><span data-stu-id="0ed37-206">MyStaticFiles/images/banner1.svg</span></span> |
| <span data-ttu-id="0ed37-207">*http://\<server_address>/StaticFiles*</span><span class="sxs-lookup"><span data-stu-id="0ed37-207">*http://\<server_address>/StaticFiles*</span></span>             |     <span data-ttu-id="0ed37-208">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="0ed37-208">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="0ed37-209">Se non esiste alcun file denominato predefinito nel *MyStaticFiles* directory *http://\<server_address > / StaticFiles* restituisce la visualizzazione con collegamenti selezionabili directory:</span><span class="sxs-lookup"><span data-stu-id="0ed37-209">If no default-named file exists in the *MyStaticFiles* directory, *http://\<server_address>/StaticFiles* returns the directory listing with clickable links:</span></span>

![Elenco di file statici](static-files/_static/db2.png)

> [!NOTE]
> <span data-ttu-id="0ed37-211">`UseDefaultFiles`e `UseDirectoryBrowser` utilizzare l'URL *http://\<server_address > / StaticFiles* senza la barra finale per attivare un lato client si viene reindirizzati *http://\<server_address > / StaticFiles /*.</span><span class="sxs-lookup"><span data-stu-id="0ed37-211">`UseDefaultFiles` and `UseDirectoryBrowser` use the URL *http://\<server_address>/StaticFiles* without the trailing slash to trigger a client-side redirect to *http://\<server_address>/StaticFiles/*.</span></span> <span data-ttu-id="0ed37-212">Si noti l'aggiunta di una barra finale.</span><span class="sxs-lookup"><span data-stu-id="0ed37-212">Notice the addition of the trailing slash.</span></span> <span data-ttu-id="0ed37-213">URL relativo all'interno dei documenti sono considerati non validi senza una barra finale.</span><span class="sxs-lookup"><span data-stu-id="0ed37-213">Relative URLs within the documents are deemed invalid without a trailing slash.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="0ed37-214">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="0ed37-214">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="0ed37-215">Il [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) classe contiene un `Mappings` proprietà che funge da un mapping di estensioni di file per i tipi di contenuto MIME.</span><span class="sxs-lookup"><span data-stu-id="0ed37-215">The [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) class contains a `Mappings` property serving as a mapping of file extensions to MIME content types.</span></span> <span data-ttu-id="0ed37-216">Nell'esempio seguente, diverse estensioni di file vengono registrate i tipi MIME noti.</span><span class="sxs-lookup"><span data-stu-id="0ed37-216">In the following sample, several file extensions are registered to known MIME types.</span></span> <span data-ttu-id="0ed37-217">Il *RTF* estensione viene sostituita, e *MP4* viene rimosso.</span><span class="sxs-lookup"><span data-stu-id="0ed37-217">The *.rtf* extension is replaced, and *.mp4* is removed.</span></span>

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

<span data-ttu-id="0ed37-218">Vedere [tipi di contenuto MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).</span><span class="sxs-lookup"><span data-stu-id="0ed37-218">See [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="0ed37-219">Tipi di contenuto non standard</span><span class="sxs-lookup"><span data-stu-id="0ed37-219">Non-standard content types</span></span>

<span data-ttu-id="0ed37-220">Il middleware di file statici riconosce i tipi di contenuto di file conosciuti quasi 400.</span><span class="sxs-lookup"><span data-stu-id="0ed37-220">The static file middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="0ed37-221">Se l'utente richiede un file di un tipo di file sconosciuto, il middleware di file statici restituisce una risposta HTTP 404 (non trovato).</span><span class="sxs-lookup"><span data-stu-id="0ed37-221">If the user requests a file of an unknown file type, the static file middleware returns a HTTP 404 (Not Found) response.</span></span> <span data-ttu-id="0ed37-222">Se è abilitata l'esplorazione directory, viene visualizzato un collegamento al file.</span><span class="sxs-lookup"><span data-stu-id="0ed37-222">If directory browsing is enabled, a link to the file is displayed.</span></span> <span data-ttu-id="0ed37-223">L'URI restituisce un errore HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="0ed37-223">The URI returns an HTTP 404 error.</span></span>

<span data-ttu-id="0ed37-224">Il codice seguente consente serve tipi sconosciuti ed esegue il rendering di file sconosciute come un'immagine:</span><span class="sxs-lookup"><span data-stu-id="0ed37-224">The following code enables serving unknown types and renders the unknown file as an image:</span></span>

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="0ed37-225">Con il codice precedente, una richiesta per un file con un tipo di contenuto sconosciuto viene restituita come un'immagine.</span><span class="sxs-lookup"><span data-stu-id="0ed37-225">With the preceding code, a request for a file with an unknown content type is returned as an image.</span></span>

> [!WARNING]
> <span data-ttu-id="0ed37-226">Abilitazione [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) costituisce un rischio di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="0ed37-226">Enabling [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) is a security risk.</span></span> <span data-ttu-id="0ed37-227">È disabilitato per impostazione predefinita e il relativo uso è sconsigliato.</span><span class="sxs-lookup"><span data-stu-id="0ed37-227">It's disabled by default, and its use is discouraged.</span></span> <span data-ttu-id="0ed37-228">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) offre un'alternativa più sicura per servire i file con estensione non standard.</span><span class="sxs-lookup"><span data-stu-id="0ed37-228">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="0ed37-229">Considerazioni</span><span class="sxs-lookup"><span data-stu-id="0ed37-229">Considerations</span></span>

> [!WARNING]
> <span data-ttu-id="0ed37-230">`UseDirectoryBrowser`e `UseStaticFiles` possono verificarsi perdite di informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="0ed37-230">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="0ed37-231">È consigliabile disabilitare directory esplorazione nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="0ed37-231">Disabling directory browsing in production is highly recommended.</span></span> <span data-ttu-id="0ed37-232">Leggere attentamente le directory vengono abilitate tramite `UseStaticFiles` o `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="0ed37-232">Carefully review which directories are enabled via `UseStaticFiles` or `UseDirectoryBrowser`.</span></span> <span data-ttu-id="0ed37-233">L'intera directory e relative sottodirectory diventano accessibili pubblicamente.</span><span class="sxs-lookup"><span data-stu-id="0ed37-233">The entire directory and its sub-directories become publicly accessible.</span></span> <span data-ttu-id="0ed37-234">Archivio file adatto per essere distribuito al pubblico in una directory dedicata, ad esempio  *\<content_root > / wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="0ed37-234">Store files suitable for serving to the public in a dedicated directory, such as *\<content_root>/wwwroot*.</span></span> <span data-ttu-id="0ed37-235">Questi file devono essere distinti dalle visualizzazioni MVC, pagine Razor (2. x solo), i file di configurazione e così via.</span><span class="sxs-lookup"><span data-stu-id="0ed37-235">Separate these files from MVC views, Razor Pages (2.x only), configuration files, etc.</span></span>

* <span data-ttu-id="0ed37-236">Gli URL per il contenuto esposti con `UseDirectoryBrowser` e `UseStaticFiles` sono soggetti a distinzione maiuscole/minuscole e restrizioni relative ai caratteri del file system sottostante.</span><span class="sxs-lookup"><span data-stu-id="0ed37-236">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of the underlying file system.</span></span> <span data-ttu-id="0ed37-237">Ad esempio, Windows viene fatta distinzione tra maiuscole e minuscole&mdash;Mac e Linux non sono.</span><span class="sxs-lookup"><span data-stu-id="0ed37-237">For example, Windows is case insensitive&mdash;Mac and Linux aren't.</span></span>

* <span data-ttu-id="0ed37-238">Le applicazioni ASP.NET Core ospitate in IIS utilizza il [ASP.NET Core modulo (ANCM)](xref:fundamentals/servers/aspnet-core-module) per inoltrare tutte le richieste per l'app, incluse le richieste di file statici.</span><span class="sxs-lookup"><span data-stu-id="0ed37-238">ASP.NET Core apps hosted in IIS use the [ASP.NET Core Module (ANCM)](xref:fundamentals/servers/aspnet-core-module) to forward all requests to the app, including static file requests.</span></span> <span data-ttu-id="0ed37-239">Il gestore di file statici di IIS non verrà usato.</span><span class="sxs-lookup"><span data-stu-id="0ed37-239">The IIS static file handler isn't used.</span></span> <span data-ttu-id="0ed37-240">Non dispone di alcuna possibilità di gestire le richieste prima di essi è gestiti dal ANCM.</span><span class="sxs-lookup"><span data-stu-id="0ed37-240">It has no chance to handle requests before they're handled by the ANCM.</span></span>

* <span data-ttu-id="0ed37-241">Completare i passaggi seguenti in Gestione IIS per rimuovere il gestore di file statici di IIS a livello di server o un sito Web:</span><span class="sxs-lookup"><span data-stu-id="0ed37-241">Complete the following steps in IIS Manager to remove the IIS static file handler at the server or website level:</span></span>
    1. <span data-ttu-id="0ed37-242">Passare il **moduli** funzionalità.</span><span class="sxs-lookup"><span data-stu-id="0ed37-242">Navigate to the **Modules** feature.</span></span>
    1. <span data-ttu-id="0ed37-243">Selezionare **StaticFileModule** nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="0ed37-243">Select **StaticFileModule** in the list.</span></span>
    1. <span data-ttu-id="0ed37-244">Fare clic su **rimuovere** nel **azioni** barra laterale.</span><span class="sxs-lookup"><span data-stu-id="0ed37-244">Click **Remove** in the **Actions** sidebar.</span></span>

> [!WARNING]
> <span data-ttu-id="0ed37-245">Se il gestore di file statici di IIS è abilitato **e** il ANCM non è configurato correttamente, i file statici vengono forniti.</span><span class="sxs-lookup"><span data-stu-id="0ed37-245">If the IIS static file handler is enabled **and** the ANCM is configured incorrectly, static files are served.</span></span> <span data-ttu-id="0ed37-246">Ciò si verifica, ad esempio, se il *Web. config* file non è stato distribuito.</span><span class="sxs-lookup"><span data-stu-id="0ed37-246">This happens, for example, if the *web.config* file isn't deployed.</span></span>

* <span data-ttu-id="0ed37-247">Inserire i file di codice (inclusi *cs* e *. cshtml*) di fuori della app radice del progetto web.</span><span class="sxs-lookup"><span data-stu-id="0ed37-247">Place code files (including *.cs* and *.cshtml*) outside of the app project's web root.</span></span> <span data-ttu-id="0ed37-248">Una separazione logica viene pertanto creata tra il contenuto sul lato client dell'app e il codice basato su server.</span><span class="sxs-lookup"><span data-stu-id="0ed37-248">A logical separation is therefore created between the app's client-side content and server-based code.</span></span> <span data-ttu-id="0ed37-249">Ciò impedisce che il codice lato server perdita.</span><span class="sxs-lookup"><span data-stu-id="0ed37-249">This prevents server-side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0ed37-250">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="0ed37-250">Additional resources</span></span>

* [<span data-ttu-id="0ed37-251">Middleware</span><span class="sxs-lookup"><span data-stu-id="0ed37-251">Middleware</span></span>](xref:fundamentals/middleware)

* [<span data-ttu-id="0ed37-252">Introduzione a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0ed37-252">Introduction to ASP.NET Core</span></span>](xref:index)