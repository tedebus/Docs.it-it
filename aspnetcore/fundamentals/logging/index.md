---
title: Registrazione in ASP.NET Core
author: ardalis
description: "Informazioni sul framework di registrazione di ASP.NET Core. Informazioni sui provider di registrazione predefiniti e sui provider di terze parti più diffusi."
ms.author: tdykstra
manager: wpickett
ms.date: 12/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging/index
ms.openlocfilehash: 387d19af9165d4b54ce3cb1a9b04412271da6fb0
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-logging-in-aspnet-core"></a><span data-ttu-id="568de-104">Introduzione alla registrazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="568de-104">Introduction to logging in ASP.NET Core</span></span>

<span data-ttu-id="568de-105">Di [Steve Smith](https://ardalis.com/) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="568de-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="568de-106">ASP.NET Core supporta un'API di registrazione che funziona con un'ampia gamma di provider di registrazione.</span><span class="sxs-lookup"><span data-stu-id="568de-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="568de-107">I provider predefiniti consentono di inviare i log a una o più destinazioni. È possibile collegare un framework di registrazione di terze parti.</span><span class="sxs-lookup"><span data-stu-id="568de-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="568de-108">Questo articolo illustra come usare l'API e i provider di registrazione incorporati nel codice.</span><span class="sxs-lookup"><span data-stu-id="568de-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="568de-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="568de-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="568de-110">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="568de-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="568de-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="568de-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="568de-112">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="568de-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="how-to-create-logs"></a><span data-ttu-id="568de-113">Come creare log</span><span class="sxs-lookup"><span data-stu-id="568de-113">How to create logs</span></span>

<span data-ttu-id="568de-114">Per creare i log, ottenere un oggetto `ILogger` dal contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="568de-114">To create logs, get an `ILogger` object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="568de-115">Quindi chiamare i metodi di registrazione su tale oggetto logger:</span><span class="sxs-lookup"><span data-stu-id="568de-115">Then call logging methods on that logger object:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="568de-116">Questo esempio crea log con la classe `TodoController` come *categoria*.</span><span class="sxs-lookup"><span data-stu-id="568de-116">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="568de-117">Le categorie sono descritte [più avanti in questo articolo](#log-category).</span><span class="sxs-lookup"><span data-stu-id="568de-117">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="568de-118">ASP.NET Core non fornisce metodi di logger asincroni perché la registrazione deve essere così rapida che non vale la pena di usare un metodo asincrono.</span><span class="sxs-lookup"><span data-stu-id="568de-118">ASP.NET Core does not provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="568de-119">Se la situazione richiede l'uso di metodi asincroni, considerare l'ipotesi di cambiare modalità di registrazione.</span><span class="sxs-lookup"><span data-stu-id="568de-119">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="568de-120">Se l'archivio dati è lento, scrivere i messaggi di log prima in un archivio veloce, quindi spostarli in un archivio lento in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="568de-120">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="568de-121">Registrare ad esempio in una coda di messaggi che viene letta e salvata in modo permanente in un archivio lento da un altro processo.</span><span class="sxs-lookup"><span data-stu-id="568de-121">For example, log to a message queue that is read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="568de-122">Come aggiungere provider</span><span class="sxs-lookup"><span data-stu-id="568de-122">How to add providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="568de-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="568de-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="568de-124">Un provider di registrazione recupera i messaggi creati dall'utente con un oggetto `ILogger` e li visualizza o li archivia.</span><span class="sxs-lookup"><span data-stu-id="568de-124">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="568de-125">Il provider Console, ad esempio, visualizza i messaggi nella console e il provider del Servizio app di Azure può archiviarli nell'archivio BLOB di Azure.</span><span class="sxs-lookup"><span data-stu-id="568de-125">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="568de-126">Per usare un provider, chiamare il metodo di estensione `Add<ProviderName>` del provider in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="568de-126">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="568de-127">Il modello di progetto predefinito consente la registrazione con il metodo [CreateDefaultBuilder](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___):</span><span class="sxs-lookup"><span data-stu-id="568de-127">The default project template enables logging with the [CreateDefaultBuilder](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) method:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="568de-128">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="568de-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="568de-129">Un provider di registrazione recupera i messaggi creati dall'utente con un oggetto `ILogger` e li visualizza o li archivia.</span><span class="sxs-lookup"><span data-stu-id="568de-129">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="568de-130">Il provider Console, ad esempio, visualizza i messaggi nella console e il provider del Servizio app di Azure può archiviarli nell'archivio BLOB di Azure.</span><span class="sxs-lookup"><span data-stu-id="568de-130">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="568de-131">Per usare un provider, installare il relativo pacchetto NuGet e chiamare il metodo di estensione del provider in un'istanza di `ILoggerFactory`, come illustrato nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="568de-131">To use a provider, install its NuGet package and call the provider's extension method on an instance of `ILoggerFactory`, as shown in the following example.</span></span>

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="568de-132">L'[inserimento delle dipendenze](xref:fundamentals/dependency-injection) di ASP.NET Core fornisce l'istanza di `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="568de-132">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="568de-133">I metodi di estensione `AddConsole` e `AddDebug` sono definiti nei pacchetti [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) e [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/).</span><span class="sxs-lookup"><span data-stu-id="568de-133">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="568de-134">Ogni metodo di estensione chiama il metodo `ILoggerFactory.AddProvider` passando un'istanza del provider.</span><span class="sxs-lookup"><span data-stu-id="568de-134">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="568de-135">L'applicazione di esempio di questo articolo aggiunge provider di registrazione nel metodo `Configure` della classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="568de-135">The sample application for this article adds logging providers in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="568de-136">Per ottenere l'output della registrazione dal codice eseguito in precedenza, aggiungere i provider di registrazione nel costruttore della classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="568de-136">If you want to get log output from code that executes earlier, add logging providers in the `Startup` class constructor instead.</span></span> 

---

<span data-ttu-id="568de-137">Altre informazioni su ciascun [provider di registrazione incorporato](#built-in-logging-providers) e i collegamenti ai [provider di registrazione di terze parti](#third-party-logging-providers) sono disponibili più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="568de-137">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="568de-138">Esempio di output di registrazione</span><span class="sxs-lookup"><span data-stu-id="568de-138">Sample logging output</span></span>

<span data-ttu-id="568de-139">Con il codice di esempio illustrato nella sezione precedente si visualizzeranno i log nella console durante l'esecuzione dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="568de-139">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="568de-140">Ecco un esempio di output della console:</span><span class="sxs-lookup"><span data-stu-id="568de-140">Here's an example of console output:</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```
 
<span data-ttu-id="568de-141">Questi log sono stati creati in `http://localhost:5000/api/todo/0`, attivando l'esecuzione di entrambe le chiamate a `ILogger` illustrate nella sezione precedente.</span><span class="sxs-lookup"><span data-stu-id="568de-141">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="568de-142">Ecco un esempio degli stessi log nella finestra Debug quando si esegue l'applicazione di esempio in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="568de-142">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="568de-143">I log che sono stati creati con le chiamate a `ILogger` illustrate nella sezione precedente iniziano con "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="568de-143">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="568de-144">I log che iniziano con le categorie "Microsoft" sono di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="568de-144">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="568de-145">ASP.NET Core stesso e il codice dell'applicazione usano la stessa API di registrazione e gli stessi provider di registrazione.</span><span class="sxs-lookup"><span data-stu-id="568de-145">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="568de-146">Il resto di questo articolo spiega alcuni dettagli e le opzioni per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="568de-146">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="568de-147">Pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="568de-147">NuGet packages</span></span>

<span data-ttu-id="568de-148">Le interfacce `ILogger` e `ILoggerFactory` si trovano in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) e le loro implementazioni predefinite si trovano in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span><span class="sxs-lookup"><span data-stu-id="568de-148">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="568de-149">Categoria dei log</span><span class="sxs-lookup"><span data-stu-id="568de-149">Log category</span></span>

<span data-ttu-id="568de-150">Ogni log creato include una *categoria*.</span><span class="sxs-lookup"><span data-stu-id="568de-150">A *category* is included with each log that you create.</span></span> <span data-ttu-id="568de-151">La categoria viene specificata quando si crea un oggetto `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="568de-151">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="568de-152">La categoria può essere qualsiasi stringa, ma una convenzione prevede l'uso del nome completo della classe da cui vengono scritti i log.</span><span class="sxs-lookup"><span data-stu-id="568de-152">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="568de-153">Ad esempio: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="568de-153">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="568de-154">È possibile specificare la categoria sotto forma di stringa o usare un metodo di estensione che deriva la categoria dal tipo.</span><span class="sxs-lookup"><span data-stu-id="568de-154">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="568de-155">Per specificare la categoria sotto forma di stringa, chiamare `CreateLogger` su un'istanza di `ILoggerFactory`, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="568de-155">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="568de-156">In genere è più semplice usare `ILogger<T>`, come nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="568de-156">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="568de-157">Ciò equivale a chiamare `CreateLogger` con il nome completo del tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="568de-157">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="568de-158">Livello di registrazione</span><span class="sxs-lookup"><span data-stu-id="568de-158">Log level</span></span>

<span data-ttu-id="568de-159">Ogni volta che si scrive un log, si specifica il relativo [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="568de-159">Each time you write a log, you specify its [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="568de-160">Il livello di registrazione indica il livello di gravità o priorità.</span><span class="sxs-lookup"><span data-stu-id="568de-160">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="568de-161">È ad esempio possibile scrivere un log `Information` quando un metodo termina normalmente, un log `Warning` quando un metodo restituisce un codice di errore 404 e un log `Error` quando viene rilevata un'eccezione imprevista.</span><span class="sxs-lookup"><span data-stu-id="568de-161">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="568de-162">Nell'esempio di codice seguente i nomi dei metodi (ad esempio `LogWarning`) specificano il livello di registrazione.</span><span class="sxs-lookup"><span data-stu-id="568de-162">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="568de-163">Il primo parametro è l'[ID dell'evento di registrazione](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="568de-163">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="568de-164">Il secondo parametro è un [modello di messaggio](#log-message-template) con segnaposto per i valori degli argomenti forniti dai rimanenti parametri del metodo.</span><span class="sxs-lookup"><span data-stu-id="568de-164">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="568de-165">I parametri del metodo sono descritti in maggiore dettaglio in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="568de-165">The method parameters are explained in more detail later in this article.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="568de-166">I metodi di registrazione che includono il livello nel nome del metodo sono i [metodi di estensione di ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="568de-166">Log methods that include the level in the method name are [extension methods for ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="568de-167">Questi metodi chiamano in background un metodo `Log` che accetta un parametro `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="568de-167">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="568de-168">È possibile chiamare il metodo `Log` direttamente anziché chiamare uno di questi metodi di estensione, ma la sintassi è relativamente complessa.</span><span class="sxs-lookup"><span data-stu-id="568de-168">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="568de-169">Per altre informazioni, vedere l'[interfaccia ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) e il [codice sorgente delle estensioni del logger](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="568de-169">For more information, see the [ILogger interface](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="568de-170">ASP.NET Core definisce i seguenti [livelli di registrazione](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), ordinati di seguito dal meno grave al più grave.</span><span class="sxs-lookup"><span data-stu-id="568de-170">ASP.NET Core defines the following [log levels](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="568de-171">Trace = 0</span><span class="sxs-lookup"><span data-stu-id="568de-171">Trace = 0</span></span>

  <span data-ttu-id="568de-172">Per informazioni utili solo per uno sviluppatore che esegue il debug di un problema.</span><span class="sxs-lookup"><span data-stu-id="568de-172">For information that is valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="568de-173">Questi messaggi possono contenere dati sensibili dell'applicazione e pertanto non devono essere abilitati in un ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="568de-173">These messages may contain sensitive application data and so should not be enabled in a production environment.</span></span> <span data-ttu-id="568de-174">*Disattivato per impostazione predefinita.*</span><span class="sxs-lookup"><span data-stu-id="568de-174">*Disabled by default.*</span></span> <span data-ttu-id="568de-175">Esempio: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="568de-175">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="568de-176">Debug = 1</span><span class="sxs-lookup"><span data-stu-id="568de-176">Debug = 1</span></span>

  <span data-ttu-id="568de-177">Per informazioni con utilità a breve termine durante lo sviluppo e il debug.</span><span class="sxs-lookup"><span data-stu-id="568de-177">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="568de-178">Esempio: `Entering method Configure with flag set to true.` In genere non si abilitano i livelli di registrazione `Debug` nell'ambiente di produzione, a meno che non occorra risolvere problemi, per l'elevato volume delle registrazioni.</span><span class="sxs-lookup"><span data-stu-id="568de-178">Example: `Entering method Configure with flag set to true.` You typically would not enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="568de-179">Information = 2</span><span class="sxs-lookup"><span data-stu-id="568de-179">Information = 2</span></span>

  <span data-ttu-id="568de-180">Per tenere traccia del flusso generale dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="568de-180">For tracking the general flow of the application.</span></span> <span data-ttu-id="568de-181">Queste registrazioni hanno in genere un valore a lungo termine.</span><span class="sxs-lookup"><span data-stu-id="568de-181">These logs typically have some long-term value.</span></span> <span data-ttu-id="568de-182">Esempio: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="568de-182">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="568de-183">Warning = 3</span><span class="sxs-lookup"><span data-stu-id="568de-183">Warning = 3</span></span>

  <span data-ttu-id="568de-184">Per gli eventi imprevisti o anomali del flusso dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="568de-184">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="568de-185">Potrebbero includere errori o altre condizioni che non provocano l'arresto dell'applicazione, ma che potrebbe essere necessario analizzare.</span><span class="sxs-lookup"><span data-stu-id="568de-185">These may include errors or other conditions that do not cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="568de-186">Le eccezioni gestite sono una situazione comune in cui usare il livello di registrazione `Warning`.</span><span class="sxs-lookup"><span data-stu-id="568de-186">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="568de-187">Esempio: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="568de-187">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="568de-188">Error = 4</span><span class="sxs-lookup"><span data-stu-id="568de-188">Error = 4</span></span>

  <span data-ttu-id="568de-189">Per errori ed eccezioni che non possono essere gestiti.</span><span class="sxs-lookup"><span data-stu-id="568de-189">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="568de-190">Questi messaggi indicano un errore nell'operazione (ad esempio la richiesta HTTP corrente) o nell'attività corrente, non un errore a livello di applicazione.</span><span class="sxs-lookup"><span data-stu-id="568de-190">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="568de-191">Messaggio di registrazione di esempio:`Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="568de-191">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="568de-192">Critical = 5</span><span class="sxs-lookup"><span data-stu-id="568de-192">Critical = 5</span></span>

  <span data-ttu-id="568de-193">Per gli errori che richiedono attenzione immediata.</span><span class="sxs-lookup"><span data-stu-id="568de-193">For failures that require immediate attention.</span></span> <span data-ttu-id="568de-194">Esempi: scenari di perdita di dati, spazio su disco insufficiente.</span><span class="sxs-lookup"><span data-stu-id="568de-194">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="568de-195">È possibile usare il livello di registrazione per controllare la quantità di output scritto su un supporto di archiviazione specifico o in una finestra.</span><span class="sxs-lookup"><span data-stu-id="568de-195">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="568de-196">Ad esempio, nell'ambiente di produzione è consigliabile che tutti i log di livello `Information` e inferiore passino a un archivio dati di volume e che tutti i log di livello `Warning` e superiore passino a un archivio dati di valori.</span><span class="sxs-lookup"><span data-stu-id="568de-196">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="568de-197">Durante lo sviluppo, in genere è possibile inviare i log di livello `Warning` o con gravità superiore alla console.</span><span class="sxs-lookup"><span data-stu-id="568de-197">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="568de-198">Per la risoluzione dei problemi, è possibile aggiungere il livello `Debug`.</span><span class="sxs-lookup"><span data-stu-id="568de-198">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="568de-199">La sezione [Filtro dei log](#log-filtering) più avanti in questo articolo descrive come controllare quali livelli di registrazione gestisce un provider.</span><span class="sxs-lookup"><span data-stu-id="568de-199">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="568de-200">Il framework di ASP.NET Core scrive log di livello `Debug` per gli eventi del framework.</span><span class="sxs-lookup"><span data-stu-id="568de-200">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="568de-201">Gli esempi visti in precedenza in questo articolo escludono i log al di sotto del livello `Information`, quindi non compare alcun log di livello `Debug`.</span><span class="sxs-lookup"><span data-stu-id="568de-201">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="568de-202">Ecco un esempio di log della console se si esegue l'applicazione di esempio configurata per visualizzare i log di livello `Debug` e superiore per il provider della console.</span><span class="sxs-lookup"><span data-stu-id="568de-202">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a><span data-ttu-id="568de-203">ID evento di registrazione</span><span class="sxs-lookup"><span data-stu-id="568de-203">Log event ID</span></span>

<span data-ttu-id="568de-204">Ogni volta che si scrive un log, è possibile specificare il relativo *ID evento*.</span><span class="sxs-lookup"><span data-stu-id="568de-204">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="568de-205">L'app di esempio esegue questa operazione tramite una classe `LoggingEvents` definita a livello locale:</span><span class="sxs-lookup"><span data-stu-id="568de-205">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="568de-206">Un ID evento è un valore intero che può essere usato per associare tra loro un set di eventi registrati.</span><span class="sxs-lookup"><span data-stu-id="568de-206">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="568de-207">Ad esempio, la registrazione dell'aggiunta di un elemento al carrello potrebbe avere ID evento 1000 e la registrazione per il completamento di un acquisto potrebbe avere ID evento 1001.</span><span class="sxs-lookup"><span data-stu-id="568de-207">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="568de-208">Nella registrazione dell'output, l'ID evento può essere archiviato in un campo o incluso nel messaggio di testo, a seconda del provider.</span><span class="sxs-lookup"><span data-stu-id="568de-208">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="568de-209">Il provider Debug non visualizza gli ID evento, mentre il provider Console li visualizza tra parentesi quadre dopo la categoria:</span><span class="sxs-lookup"><span data-stu-id="568de-209">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="568de-210">Modello di messaggio di registrazione</span><span class="sxs-lookup"><span data-stu-id="568de-210">Log message template</span></span>

<span data-ttu-id="568de-211">Ogni volta che si scrive un messaggio di registrazione, si fornisce un modello di messaggio.</span><span class="sxs-lookup"><span data-stu-id="568de-211">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="568de-212">Il modello di messaggio può essere una stringa o può contenere segnaposto denominati in cui vengono inseriti i valori degli argomenti.</span><span class="sxs-lookup"><span data-stu-id="568de-212">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="568de-213">Il modello non è una stringa formattata e i segnaposto devono essere denominati, non numerati.</span><span class="sxs-lookup"><span data-stu-id="568de-213">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="568de-214">L'ordine dei segnaposto, non il loro nome, determina i parametri da usare per fornire i valori corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="568de-214">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="568de-215">Se si ha il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="568de-215">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="568de-216">Il messaggio di registrazione risultante è simile a:</span><span class="sxs-lookup"><span data-stu-id="568de-216">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="568de-217">Il framework di registrazione formatta i messaggi in questo modo per consentire ai provider di registrazione di implementare la [registrazione semantica, nota anche come registrazione strutturata](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="568de-217">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="568de-218">Poiché gli argomenti stessi vengono passati al sistema di registrazione, non solo il modello di messaggio formattato, i provider di registrazione possono archiviare i valori dei parametri come campi oltre al modello di messaggio.</span><span class="sxs-lookup"><span data-stu-id="568de-218">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="568de-219">Se si indirizza l'output del log all'archiviazione tabelle di Azure e il metodo logger ha l'aspetto seguente:</span><span class="sxs-lookup"><span data-stu-id="568de-219">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="568de-220">Ogni entità della tabella di Azure può avere le proprietà `ID` e `RequestTime`, il che semplifica le query sui dati del log.</span><span class="sxs-lookup"><span data-stu-id="568de-220">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="568de-221">È possibile trovare tutti i log entro un determinato intervallo `RequestTime` senza che sia necessario analizzare il tempo fuori dal messaggio di testo.</span><span class="sxs-lookup"><span data-stu-id="568de-221">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="568de-222">Registrazione delle eccezioni</span><span class="sxs-lookup"><span data-stu-id="568de-222">Logging exceptions</span></span>

<span data-ttu-id="568de-223">I metodi logger dispongono di overload che consentono di passare un'eccezione, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="568de-223">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="568de-224">Provider diversi gestiscono le informazioni sulle eccezioni in modi diversi.</span><span class="sxs-lookup"><span data-stu-id="568de-224">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="568de-225">Ecco un esempio di output del provider Debug dal codice sopra riportato.</span><span class="sxs-lookup"><span data-stu-id="568de-225">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="568de-226">Filtro dei log</span><span class="sxs-lookup"><span data-stu-id="568de-226">Log filtering</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="568de-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="568de-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="568de-228">È possibile specificare un livello di registrazione minimo per un provider e una categoria specifici oppure per tutti i provider o tutte le categorie.</span><span class="sxs-lookup"><span data-stu-id="568de-228">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="568de-229">Tutti i log sotto il livello minimo non sono passati al provider, quindi non vengono visualizzati o archiviati.</span><span class="sxs-lookup"><span data-stu-id="568de-229">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="568de-230">Se si vuole eliminare tutti i log, è possibile specificare `LogLevel.None` come livello di registrazione minimo.</span><span class="sxs-lookup"><span data-stu-id="568de-230">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="568de-231">Il valore intero di `LogLevel.None` è 6, maggiore di `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="568de-231">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="568de-232">**Creare regole di filtro nella configurazione**</span><span class="sxs-lookup"><span data-stu-id="568de-232">**Create filter rules in configuration**</span></span>

<span data-ttu-id="568de-233">I modelli di progetto creano codice che chiama `CreateDefaultBuilder` per impostare la registrazione per i provider Console e Debug.</span><span class="sxs-lookup"><span data-stu-id="568de-233">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="568de-234">Il metodo `CreateDefaultBuilder` imposta inoltre la registrazione per la ricerca della configurazione di una sezione `Logging` tramite codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="568de-234">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="568de-235">I dati di configurazione specificano i livelli di registrazione minimi in base al provider e alla categoria, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="568de-235">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/sample2/appsettings.json)]

<span data-ttu-id="568de-236">Questo codice JSON crea sei regole di filtro, una per il provider Debug, quattro per il provider Console e una che si applica a tutti i provider.</span><span class="sxs-lookup"><span data-stu-id="568de-236">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="568de-237">Più avanti si vedrà come si sceglie una sola di queste regole per ogni provider quando si crea un oggetto `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="568de-237">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="568de-238">**Regole di filtro nel codice**</span><span class="sxs-lookup"><span data-stu-id="568de-238">**Filter rules in code**</span></span>

<span data-ttu-id="568de-239">È possibile registrare le regole di filtro nel codice, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="568de-239">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="568de-240">Il secondo `AddFilter` specifica il provider Debug tramite il nome del tipo.</span><span class="sxs-lookup"><span data-stu-id="568de-240">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="568de-241">Il primo `AddFilter` si applica a tutti i provider in quanto non consente di specificare un tipo di provider.</span><span class="sxs-lookup"><span data-stu-id="568de-241">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="568de-242">**Applicazione delle regole di filtro**</span><span class="sxs-lookup"><span data-stu-id="568de-242">**How filtering rules are applied**</span></span>

<span data-ttu-id="568de-243">I dati di configurazione e il codice `AddFilter` illustrato negli esempi precedenti creano le regole indicate nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="568de-243">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="568de-244">I primi sei provengono dall'esempio di configurazione e gli ultimi due dall'esempio di codice.</span><span class="sxs-lookup"><span data-stu-id="568de-244">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="568de-245">Number</span><span class="sxs-lookup"><span data-stu-id="568de-245">Number</span></span> | <span data-ttu-id="568de-246">Provider</span><span class="sxs-lookup"><span data-stu-id="568de-246">Provider</span></span>      | <span data-ttu-id="568de-247">Categorie che iniziano con...</span><span class="sxs-lookup"><span data-stu-id="568de-247">Categories that begin with ...</span></span>          | <span data-ttu-id="568de-248">Livello di registrazione minimo</span><span class="sxs-lookup"><span data-stu-id="568de-248">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="568de-249">1</span><span class="sxs-lookup"><span data-stu-id="568de-249">1</span></span>      | <span data-ttu-id="568de-250">Debug</span><span class="sxs-lookup"><span data-stu-id="568de-250">Debug</span></span>         | <span data-ttu-id="568de-251">Tutte le categorie</span><span class="sxs-lookup"><span data-stu-id="568de-251">All categories</span></span>                          | <span data-ttu-id="568de-252">Informazioni</span><span class="sxs-lookup"><span data-stu-id="568de-252">Information</span></span>       |
| <span data-ttu-id="568de-253">2</span><span class="sxs-lookup"><span data-stu-id="568de-253">2</span></span>      | <span data-ttu-id="568de-254">Console</span><span class="sxs-lookup"><span data-stu-id="568de-254">Console</span></span>       | <span data-ttu-id="568de-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="568de-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="568de-256">Avviso</span><span class="sxs-lookup"><span data-stu-id="568de-256">Warning</span></span>           |
| <span data-ttu-id="568de-257">3</span><span class="sxs-lookup"><span data-stu-id="568de-257">3</span></span>      | <span data-ttu-id="568de-258">Console</span><span class="sxs-lookup"><span data-stu-id="568de-258">Console</span></span>       | <span data-ttu-id="568de-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="568de-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="568de-260">Debug</span><span class="sxs-lookup"><span data-stu-id="568de-260">Debug</span></span>             |
| <span data-ttu-id="568de-261">4</span><span class="sxs-lookup"><span data-stu-id="568de-261">4</span></span>      | <span data-ttu-id="568de-262">Console</span><span class="sxs-lookup"><span data-stu-id="568de-262">Console</span></span>       | <span data-ttu-id="568de-263">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="568de-263">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="568de-264">Error</span><span class="sxs-lookup"><span data-stu-id="568de-264">Error</span></span>             |
| <span data-ttu-id="568de-265">5</span><span class="sxs-lookup"><span data-stu-id="568de-265">5</span></span>      | <span data-ttu-id="568de-266">Console</span><span class="sxs-lookup"><span data-stu-id="568de-266">Console</span></span>       | <span data-ttu-id="568de-267">Tutte le categorie</span><span class="sxs-lookup"><span data-stu-id="568de-267">All categories</span></span>                          | <span data-ttu-id="568de-268">Informazioni</span><span class="sxs-lookup"><span data-stu-id="568de-268">Information</span></span>       |
| <span data-ttu-id="568de-269">6</span><span class="sxs-lookup"><span data-stu-id="568de-269">6</span></span>      | <span data-ttu-id="568de-270">Tutti i provider</span><span class="sxs-lookup"><span data-stu-id="568de-270">All providers</span></span> | <span data-ttu-id="568de-271">Tutte le categorie</span><span class="sxs-lookup"><span data-stu-id="568de-271">All categories</span></span>                          | <span data-ttu-id="568de-272">Debug</span><span class="sxs-lookup"><span data-stu-id="568de-272">Debug</span></span>             |
| <span data-ttu-id="568de-273">7</span><span class="sxs-lookup"><span data-stu-id="568de-273">7</span></span>      | <span data-ttu-id="568de-274">Tutti i provider</span><span class="sxs-lookup"><span data-stu-id="568de-274">All providers</span></span> | <span data-ttu-id="568de-275">System</span><span class="sxs-lookup"><span data-stu-id="568de-275">System</span></span>                                  | <span data-ttu-id="568de-276">Debug</span><span class="sxs-lookup"><span data-stu-id="568de-276">Debug</span></span>             |
| <span data-ttu-id="568de-277">8</span><span class="sxs-lookup"><span data-stu-id="568de-277">8</span></span>      | <span data-ttu-id="568de-278">Debug</span><span class="sxs-lookup"><span data-stu-id="568de-278">Debug</span></span>         | <span data-ttu-id="568de-279">Microsoft</span><span class="sxs-lookup"><span data-stu-id="568de-279">Microsoft</span></span>                               | <span data-ttu-id="568de-280">Traccia</span><span class="sxs-lookup"><span data-stu-id="568de-280">Trace</span></span>             |

<span data-ttu-id="568de-281">Quando si crea un oggetto `ILogger` con cui scrivere i log, l'oggetto `ILoggerFactory` seleziona una singola regola per ogni provider da applicare al logger.</span><span class="sxs-lookup"><span data-stu-id="568de-281">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="568de-282">Tutti i messaggi scritti da tale oggetto `ILogger` sono filtrati in base alle regole selezionate.</span><span class="sxs-lookup"><span data-stu-id="568de-282">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="568de-283">Tra le regole disponibili viene selezionata la regola più specifica possibile per ogni coppia di categoria e provider.</span><span class="sxs-lookup"><span data-stu-id="568de-283">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="568de-284">L'algoritmo seguente viene usato per ogni provider quando viene creato un `ILogger` per una determinata categoria:</span><span class="sxs-lookup"><span data-stu-id="568de-284">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="568de-285">Selezionare tutte le regole corrispondenti al provider o al relativo alias.</span><span class="sxs-lookup"><span data-stu-id="568de-285">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="568de-286">Se non ne vengono rilevate, selezionare tutte le regole con un provider vuoto.</span><span class="sxs-lookup"><span data-stu-id="568de-286">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="568de-287">Dal risultato del passaggio precedente, selezionare le regole con il prefisso di categoria corrispondente più lungo.</span><span class="sxs-lookup"><span data-stu-id="568de-287">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="568de-288">Se non vengono rilevate, selezionare tutte le regole che non specificano una categoria.</span><span class="sxs-lookup"><span data-stu-id="568de-288">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="568de-289">Se sono selezionate più regole, scegliere l'**ultima**.</span><span class="sxs-lookup"><span data-stu-id="568de-289">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="568de-290">Se non è selezionata alcuna regola, usare `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="568de-290">If no rules are selected, use `MinimumLevel`.</span></span>
 
<span data-ttu-id="568de-291">Si supponga ad esempio di avere l'elenco di regole precedente e di creare un oggetto `ILogger` per la categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="568de-291">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="568de-292">Per il provider Debug si applicano le regole 1, 6 e 8.</span><span class="sxs-lookup"><span data-stu-id="568de-292">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="568de-293">La regola 8 è più specifica, così sarà quella selezionata.</span><span class="sxs-lookup"><span data-stu-id="568de-293">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="568de-294">Per il provider Console si applicano le regole 3, 4, 5 e 6.</span><span class="sxs-lookup"><span data-stu-id="568de-294">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="568de-295">La regola 3 è la più specifica.</span><span class="sxs-lookup"><span data-stu-id="568de-295">Rule 3 is most specific.</span></span>

<span data-ttu-id="568de-296">Quando si creano log con un `ILogger` per la categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", i log di livello `Trace` e superiore andranno al provider Debug e i log di livello `Debug` e superiore andranno al provider Console.</span><span class="sxs-lookup"><span data-stu-id="568de-296">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="568de-297">**Alias dei provider**</span><span class="sxs-lookup"><span data-stu-id="568de-297">**Provider aliases**</span></span>

<span data-ttu-id="568de-298">È possibile usare il nome del tipo per specificare un provider nella configurazione, ma ogni provider definisce un *alias* più breve che è più facile da usare.</span><span class="sxs-lookup"><span data-stu-id="568de-298">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that is easier to use.</span></span> <span data-ttu-id="568de-299">Per i provider predefiniti, usare gli alias seguenti:</span><span class="sxs-lookup"><span data-stu-id="568de-299">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="568de-300">Console</span><span class="sxs-lookup"><span data-stu-id="568de-300">Console</span></span>
- <span data-ttu-id="568de-301">Debug</span><span class="sxs-lookup"><span data-stu-id="568de-301">Debug</span></span>
- <span data-ttu-id="568de-302">EventLog</span><span class="sxs-lookup"><span data-stu-id="568de-302">EventLog</span></span>
- <span data-ttu-id="568de-303">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="568de-303">AzureAppServices</span></span>
- <span data-ttu-id="568de-304">TraceSource</span><span class="sxs-lookup"><span data-stu-id="568de-304">TraceSource</span></span>
- <span data-ttu-id="568de-305">EventSource</span><span class="sxs-lookup"><span data-stu-id="568de-305">EventSource</span></span>

<span data-ttu-id="568de-306">**Livello minimo predefinito**</span><span class="sxs-lookup"><span data-stu-id="568de-306">**Default minimum level**</span></span>

<span data-ttu-id="568de-307">Esiste un'impostazione del livello minimo che diventa effettiva solo se non si applica alcuna regola di configurazione o codice per un provider e una categoria specifici.</span><span class="sxs-lookup"><span data-stu-id="568de-307">There is a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="568de-308">L'esempio seguente illustra come impostare il livello minimo:</span><span class="sxs-lookup"><span data-stu-id="568de-308">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="568de-309">Se non si imposta in modo esplicito il livello minimo, il valore predefinito è `Information`, che indica che i log `Trace` e `Debug` vengono ignorati.</span><span class="sxs-lookup"><span data-stu-id="568de-309">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="568de-310">**Funzioni di filtro**</span><span class="sxs-lookup"><span data-stu-id="568de-310">**Filter functions**</span></span>

<span data-ttu-id="568de-311">È possibile scrivere codice in una funzione di filtro per applicare le regole di filtro.</span><span class="sxs-lookup"><span data-stu-id="568de-311">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="568de-312">Una funzione di filtro viene richiamata per tutti i provider e le categorie a cui non sono assegnate regole tramite la configurazione o il codice.</span><span class="sxs-lookup"><span data-stu-id="568de-312">A filter function is invoked for all providers and categories that do not have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="568de-313">Il codice della funzione ha accesso al tipo di provider, alla categoria e al livello di registrazione per stabilire se un messaggio deve essere registrato.</span><span class="sxs-lookup"><span data-stu-id="568de-313">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="568de-314">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="568de-314">For example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="568de-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="568de-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="568de-316">Alcuni provider di registrazione consentono di specificare quando i log devono essere scritti in un supporto di archiviazione oppure ignorati in base al livello di registrazione e alla categoria.</span><span class="sxs-lookup"><span data-stu-id="568de-316">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="568de-317">I metodi di estensione `AddConsole` e `AddDebug` forniscono overload che consentono di passare criteri di filtro.</span><span class="sxs-lookup"><span data-stu-id="568de-317">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="568de-318">L'esempio di codice seguente fa sì che il provider Console ignori i log di livello inferiore a `Warning`, mentre il provider Debug ignora i log creati dal framework.</span><span class="sxs-lookup"><span data-stu-id="568de-318">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="568de-319">Il metodo `AddEventLog` ha un overload che accetta un'istanza di `EventLogSettings`, che può contenere una funzione di filtro nella proprietà `Filter`.</span><span class="sxs-lookup"><span data-stu-id="568de-319">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="568de-320">Il provider TraceSource non fornisce alcun overload di questo tipo, in quanto il suo livello di registrazione e altri parametri si basano sugli oggetti `SourceSwitch` e `TraceListener` in uso.</span><span class="sxs-lookup"><span data-stu-id="568de-320">The TraceSource provider does not provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="568de-321">È possibile impostare le regole di filtro per tutti i provider registrati con un'istanza di `ILoggerFactory` tramite il metodo di estensione `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="568de-321">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="568de-322">L'esempio seguente limita i log del framework (la categoria inizia con "Microsoft" o "System") agli avvisi consentendo all'app di registrare a livello debug.</span><span class="sxs-lookup"><span data-stu-id="568de-322">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="568de-323">Per usare i filtri per impedire la scrittura di tutti i log per una determinata categoria, è possibile specificare `LogLevel.None` come il livello di registrazione minimo per tale categoria.</span><span class="sxs-lookup"><span data-stu-id="568de-323">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="568de-324">Il valore intero di `LogLevel.None` è 6, maggiore di `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="568de-324">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="568de-325">Il metodo di estensione `WithFilter` viene fornito dal pacchetto NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter).</span><span class="sxs-lookup"><span data-stu-id="568de-325">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="568de-326">Il metodo restituisce una nuova istanza di `ILoggerFactory` che filtra i messaggi di registrazione passati a tutti i provider di logger registrati con esso.</span><span class="sxs-lookup"><span data-stu-id="568de-326">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="568de-327">Non si applica ad altre istanze di `ILoggerFactory`, inclusa l'istanza originale di `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="568de-327">It does not affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

---

## <a name="log-scopes"></a><span data-ttu-id="568de-328">Ambiti dei log</span><span class="sxs-lookup"><span data-stu-id="568de-328">Log scopes</span></span>

<span data-ttu-id="568de-329">È possibile raggruppare un set di operazioni logiche all'interno di un *ambito* per collegare gli stessi dati per ogni log che viene creato come parte del set.</span><span class="sxs-lookup"><span data-stu-id="568de-329">You can group a set of logical operations within a *scope* in order to attach the same data to each log that is created as part of that set.</span></span> <span data-ttu-id="568de-330">È ad esempio possibile fare in modo che ogni log creato come parte dell'elaborazione di una transazione includa l'ID della transazione.</span><span class="sxs-lookup"><span data-stu-id="568de-330">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="568de-331">Un ambito è un tipo `IDisposable` restituito dal metodo `ILogger.BeginScope<TState>` e viene mantenuto fino a quando non viene eliminato.</span><span class="sxs-lookup"><span data-stu-id="568de-331">A scope is an `IDisposable` type that is returned by the `ILogger.BeginScope<TState>` method and lasts until it is disposed.</span></span> <span data-ttu-id="568de-332">Un ambito si usa mediante il wrapping delle chiamate del logger in un blocco `using`, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="568de-332">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="568de-333">Il codice seguente abilita gli ambiti per il provider Console:</span><span class="sxs-lookup"><span data-stu-id="568de-333">The following code enables scopes for the console provider:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="568de-334">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="568de-334">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="568de-335">In *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="568de-335">In *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="568de-336">La configurazione dell'opzione del logger della console `IncludeScopes` è necessaria per abilitare la registrazione basata sull'ambito.</span><span class="sxs-lookup"><span data-stu-id="568de-336">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span> <span data-ttu-id="568de-337">La configurazione di `IncludeScopes` tramite i file di configurazione *appsettings* sarà disponibile con il rilascio di ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="568de-337">Configuration of `IncludeScopes` using *appsettings* configuration files will be available with the release of ASP.NET Core 2.1.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="568de-338">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="568de-338">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="568de-339">In *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="568de-339">In *Startup.cs*:</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

<span data-ttu-id="568de-340">Ogni messaggio di registrazione include le informazioni sull'ambito:</span><span class="sxs-lookup"><span data-stu-id="568de-340">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="568de-341">Provider di registrazione predefiniti</span><span class="sxs-lookup"><span data-stu-id="568de-341">Built-in logging providers</span></span>

<span data-ttu-id="568de-342">ASP.NET Core include i provider seguenti:</span><span class="sxs-lookup"><span data-stu-id="568de-342">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="568de-343">Console</span><span class="sxs-lookup"><span data-stu-id="568de-343">Console</span></span>](#console)
* [<span data-ttu-id="568de-344">Debug</span><span class="sxs-lookup"><span data-stu-id="568de-344">Debug</span></span>](#debug)
* [<span data-ttu-id="568de-345">EventSource</span><span class="sxs-lookup"><span data-stu-id="568de-345">EventSource</span></span>](#eventsource)
* [<span data-ttu-id="568de-346">EventLog</span><span class="sxs-lookup"><span data-stu-id="568de-346">EventLog</span></span>](#eventlog)
* [<span data-ttu-id="568de-347">TraceSource</span><span class="sxs-lookup"><span data-stu-id="568de-347">TraceSource</span></span>](#tracesource)
* [<span data-ttu-id="568de-348">Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="568de-348">Azure App Service</span></span>](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a><span data-ttu-id="568de-349">Provider Console</span><span class="sxs-lookup"><span data-stu-id="568de-349">The console provider</span></span>

<span data-ttu-id="568de-350">Il pacchetto del provider [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) invia l'output della registrazione alla console.</span><span class="sxs-lookup"><span data-stu-id="568de-350">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="568de-351">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="568de-351">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="568de-352">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="568de-352">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="568de-353">[Gli overload di AddConsole](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) consentono di passare un livello di registrazione minimo, una funzione di filtro e un valore booleano che indica se gli ambiti sono supportati.</span><span class="sxs-lookup"><span data-stu-id="568de-353">[AddConsole overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="568de-354">Un'altra opzione consiste nel passare un oggetto `IConfiguration`, che può specificare livelli di registrazione e il supporto di ambiti.</span><span class="sxs-lookup"><span data-stu-id="568de-354">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="568de-355">Se si intende usare il provider di console nell'ambiente di produzione, tenere presente che ha un impatto significativo sulle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="568de-355">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="568de-356">Quando si crea un nuovo progetto in Visual Studio, il metodo `AddConsole` è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="568de-356">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="568de-357">Questo codice fa riferimento alla sezione `Logging` del file *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="568de-357">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/sample//appsettings.json)]

<span data-ttu-id="568de-358">Le impostazioni visualizzate limitano i log del framework agli avvisi, consentendo all'app di registrare a livello di debug, come descritto nella sezione [Filtri dei log](#log-filtering).</span><span class="sxs-lookup"><span data-stu-id="568de-358">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="568de-359">Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="568de-359">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

---

<a id="debug"></a>
### <a name="the-debug-provider"></a><span data-ttu-id="568de-360">Provider Debug</span><span class="sxs-lookup"><span data-stu-id="568de-360">The Debug provider</span></span>

<span data-ttu-id="568de-361">Il pacchetto del provider [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) scrive l'output della registrazione usando la classe [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) (chiamate del metodo `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="568de-361">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="568de-362">In Linux, questo provider scrive i log in */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="568de-362">On Linux, this provider writes logs to */var/log/message*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="568de-363">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="568de-363">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="568de-364">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="568de-364">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="568de-365">Gli [overload di AddDebug](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) consentono passare un livello di registrazione minimo o una funzione di filtro.</span><span class="sxs-lookup"><span data-stu-id="568de-365">[AddDebug overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a><span data-ttu-id="568de-366">Provider EventSource</span><span class="sxs-lookup"><span data-stu-id="568de-366">The EventSource provider</span></span>

<span data-ttu-id="568de-367">Per le app destinate a ASP.NET Core 1.1.0 o versione successiva, il pacchetto di provider [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) può implementare la traccia degli eventi.</span><span class="sxs-lookup"><span data-stu-id="568de-367">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="568de-368">In Windows, usa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="568de-368">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="568de-369">Il provider è multipiattaforma, ma non sono ancora disponibili gli strumenti di raccolta e visualizzazione degli eventi per Linux o macOS.</span><span class="sxs-lookup"><span data-stu-id="568de-369">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="568de-370">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="568de-370">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="568de-371">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="568de-371">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

<span data-ttu-id="568de-372">Un buon metodo per raccogliere e visualizzare i log consiste nell'usare l'[utilità PerfView](https://www.microsoft.com/download/details.aspx?id=28567).</span><span class="sxs-lookup"><span data-stu-id="568de-372">A good way to collect and view logs is to use the [PerfView utility](https://www.microsoft.com/download/details.aspx?id=28567).</span></span> <span data-ttu-id="568de-373">Sono disponibili altri strumenti per la visualizzazione dei log ETW, ma PerfView fornisce un'esperienza ottimale per l'uso con gli eventi ETW generati da ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="568de-373">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="568de-374">Per configurare PerfView per la raccolta degli eventi registrati da questo provider, aggiungere la stringa `*Microsoft-Extensions-Logging` nell'elenco **Provider aggiuntivi**</span><span class="sxs-lookup"><span data-stu-id="568de-374">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="568de-375">(non dimenticare l'asterisco all'inizio della stringa).</span><span class="sxs-lookup"><span data-stu-id="568de-375">(Don't miss the asterisk at the start of the string.)</span></span>

![Provider aggiuntivi di PerfView](index/_static/perfview-additional-providers.png)

<span data-ttu-id="568de-377">L'acquisizione di eventi su Nano Server richiede operazioni di configurazione aggiuntive:</span><span class="sxs-lookup"><span data-stu-id="568de-377">Capturing events on Nano Server requires some additional setup:</span></span>

* <span data-ttu-id="568de-378">Connettere la sessione remota di PowerShell a Nano Server:</span><span class="sxs-lookup"><span data-stu-id="568de-378">Connect PowerShell remoting to the Nano Server:</span></span>

  ```powershell
  Enter-PSSession [name]
  ```

* <span data-ttu-id="568de-379">Creare una sessione ETW:</span><span class="sxs-lookup"><span data-stu-id="568de-379">Create an ETW session:</span></span>

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* <span data-ttu-id="568de-380">Aggiungere i provider ETW per [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers), ASP.NET Core e altri in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="568de-380">Add ETW providers for [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers), ASP.NET Core, and others as needed.</span></span> <span data-ttu-id="568de-381">Il GUID del provider ASP.NET Core è `3ac73b97-af73-50e9-0822-5da4367920d0`.</span><span class="sxs-lookup"><span data-stu-id="568de-381">The ASP.NET Core provider GUID is `3ac73b97-af73-50e9-0822-5da4367920d0`.</span></span> 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* <span data-ttu-id="568de-382">Eseguire il sito e tutte le azioni per cui si desidera registrare le informazioni.</span><span class="sxs-lookup"><span data-stu-id="568de-382">Run the site and do whatever actions you want tracing information for.</span></span>

* <span data-ttu-id="568de-383">Arrestare la sessione di traccia al termine:</span><span class="sxs-lookup"><span data-stu-id="568de-383">Stop the tracing session when you're finished:</span></span>

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

<span data-ttu-id="568de-384">Il file risultante *C:\trace.etl* può essere analizzato con PerfView come in altre edizioni di Windows.</span><span class="sxs-lookup"><span data-stu-id="568de-384">The resulting *C:\trace.etl* file can be analyzed with PerfView as on other editions of Windows.</span></span>

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a><span data-ttu-id="568de-385">Provider EventLog di Windows</span><span class="sxs-lookup"><span data-stu-id="568de-385">The Windows EventLog provider</span></span>

<span data-ttu-id="568de-386">Il pacchetto di provider [Microsoft.Extensions.Logging.AventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) invia l'output del log al registro eventi di Windows.</span><span class="sxs-lookup"><span data-stu-id="568de-386">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="568de-387">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="568de-387">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="568de-388">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="568de-388">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="568de-389">Gli [overload di AddEventLog](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) consentono di passare `EventLogSettings` o un livello di registrazione minimo.</span><span class="sxs-lookup"><span data-stu-id="568de-389">[AddEventLog overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a><span data-ttu-id="568de-390">Provider TraceSource</span><span class="sxs-lookup"><span data-stu-id="568de-390">The TraceSource provider</span></span>

<span data-ttu-id="568de-391">Il pacchetto di provider [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) usa le librerie e i provider di [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource).</span><span class="sxs-lookup"><span data-stu-id="568de-391">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="568de-392">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="568de-392">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="568de-393">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="568de-393">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

<span data-ttu-id="568de-394">Gli [overload di AddTraceSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) consentono di passare un cambio di origine e un listener di traccia.</span><span class="sxs-lookup"><span data-stu-id="568de-394">[AddTraceSource overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="568de-395">Per usare questo provider, un'applicazione deve essere eseguita su di .NET Framework (anziché su .NET Core).</span><span class="sxs-lookup"><span data-stu-id="568de-395">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="568de-396">Il provider consente di instradare i messaggi a vari [listener](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners), come il listener [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr) usato nell'applicazione di esempio.</span><span class="sxs-lookup"><span data-stu-id="568de-396">The provider lets you route messages to a variety of [listeners](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="568de-397">L'esempio seguente configura un provider `TraceSource` che registra messaggi `Warning` e di livello superiore nella finestra della console.</span><span class="sxs-lookup"><span data-stu-id="568de-397">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a><span data-ttu-id="568de-398">Provider del Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="568de-398">The Azure App Service provider</span></span>

<span data-ttu-id="568de-399">Il pacchetto di provider [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) scrive i log in file di testo nel file system di un'app del Servizio app di Azure e nell'[archivio di BLOB](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in un account di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="568de-399">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="568de-400">Il provider è disponibile solo per le app destinate ad ASP.NET Core 1.1.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="568de-400">The provider is available only for apps that target ASP.NET Core 1.1.0 or higher.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="568de-401">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="568de-401">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="568de-402">Se la destinazione è .NET Core, non è necessario installare il pacchetto di provider o chiamare `AddAzureWebAppDiagnostics` in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="568de-402">If targeting .NET Core, you don't have to install the provider package or explicitly call `AddAzureWebAppDiagnostics`.</span></span> <span data-ttu-id="568de-403">Il provider è automaticamente disponibile per l'app quando la si distribuisce nel di Servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="568de-403">The provider is automatically available to your app when you deploy the app to Azure App Service.</span></span>

<span data-ttu-id="568de-404">Se la destinazione è .NET Framework, aggiungere il pacchetto di provider al progetto e richiamare `AddAzureWebAppDiagnostics`:</span><span class="sxs-lookup"><span data-stu-id="568de-404">If targeting .NET Framework, add the provider package to your project and invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="568de-405">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="568de-405">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="568de-406">Un overload di `AddAzureWebAppDiagnostics` consente di passare [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs) con cui è possibile eseguire l'override delle impostazioni predefinite, ad esempio il modello di output di registrazione, il nome di BLOB e il limite di dimensioni di file.</span><span class="sxs-lookup"><span data-stu-id="568de-406">An `AddAzureWebAppDiagnostics` overload lets you pass in [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs) with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="568de-407">(Il *modello di output* è un modello di messaggio che viene applicato a tutti i log oltre quello specificato dall'utente quando si chiama un metodo `ILogger`.)</span><span class="sxs-lookup"><span data-stu-id="568de-407">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

---

<span data-ttu-id="568de-408">Quando si distribuisce un'app del Servizio app, l'applicazione rispetta le impostazioni della sezione dei [log di diagnostica](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) dalla pagina del **Servizio app** del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="568de-408">When you deploy to an App Service app, your application honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="568de-409">Quando si modificano queste impostazioni, le modifiche hanno effetto immediatamente senza la necessità di riavviare l'app o ridistribuire codice.</span><span class="sxs-lookup"><span data-stu-id="568de-409">When you change those settings, the changes take effect immediately without requiring that you restart the app or redeploy code to it.</span></span> 

![Impostazioni di registrazione di Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="568de-411">Il percorso predefinito per i file di log è nella cartella *D:\\home\\LogFiles\\Application* e il nome di file predefinito è *diagnostics-aaaammgg.txt*.</span><span class="sxs-lookup"><span data-stu-id="568de-411">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="568de-412">Il limite predefinito per le dimensioni del file è 10 MB e il numero massimo predefinito di file conservati è 2.</span><span class="sxs-lookup"><span data-stu-id="568de-412">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="568de-413">Il nome BLOB predefinito è *{nome-app}{timestamp}/aaaa/mm/gg/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="568de-413">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="568de-414">Per altre informazioni sul comportamento predefinito, vedere [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span><span class="sxs-lookup"><span data-stu-id="568de-414">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span></span>

<span data-ttu-id="568de-415">Il provider funziona solo quando il progetto viene eseguito nell'ambiente di Azure.</span><span class="sxs-lookup"><span data-stu-id="568de-415">The provider only works when your project runs in the Azure environment.</span></span> <span data-ttu-id="568de-416">Non ha alcun effetto quando si esegue localmente, in quanto non scrive nulla in file locali o in archivi di sviluppo locali per i BLOB.</span><span class="sxs-lookup"><span data-stu-id="568de-416">It has no effect when you run locally &mdash; it does not write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="568de-417">Provider di registrazione di terze parti</span><span class="sxs-lookup"><span data-stu-id="568de-417">Third-party logging providers</span></span>

<span data-ttu-id="568de-418">Ecco alcuni framework di registrazione di terze parti che usano ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="568de-418">Here are some third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="568de-419">[elmah.IO](https://github.com/elmahio/Elmah.Io.Extensions.Logging): provider per il servizio Elmah.Io</span><span class="sxs-lookup"><span data-stu-id="568de-419">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) - provider for the Elmah.Io service</span></span>

* <span data-ttu-id="568de-420">[JSNLog](http://jsnlog.com): registra le eccezioni JavaScript e altri eventi del lato client nel log sul lato server.</span><span class="sxs-lookup"><span data-stu-id="568de-420">[JSNLog](http://jsnlog.com) - logs JavaScript exceptions and other client-side events in your server-side log.</span></span>

* <span data-ttu-id="568de-421">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging): provider per il servizio Loggr</span><span class="sxs-lookup"><span data-stu-id="568de-421">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) - provider for the Loggr service</span></span>

* <span data-ttu-id="568de-422">[NLog](https://github.com/NLog/NLog.Extensions.Logging): provider per la libreria NLog</span><span class="sxs-lookup"><span data-stu-id="568de-422">[NLog](https://github.com/NLog/NLog.Extensions.Logging) - provider for the NLog library</span></span>

* <span data-ttu-id="568de-423">[Serilog](https://github.com/serilog/serilog-extensions-logging): provider per la libreria Serilog</span><span class="sxs-lookup"><span data-stu-id="568de-423">[Serilog](https://github.com/serilog/serilog-extensions-logging) - provider for the Serilog library</span></span>

<span data-ttu-id="568de-424">Alcuni framework di terze parti possono eseguire la [registrazione semantica, nota anche come registrazione strutturata](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="568de-424">Some third-party frameworks can do [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="568de-425">L'uso di un framework di terze parti è simile all'utilizzo di uno dei provider predefiniti: si aggiunge un pacchetto NuGet al progetto e si chiama un metodo di estensione su `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="568de-425">Using a third-party framework is similar to using one of the built-in providers: add a NuGet package to your project and call an extension method on `ILoggerFactory`.</span></span> <span data-ttu-id="568de-426">Per altre informazioni, vedere la documentazione di ciascun framework.</span><span class="sxs-lookup"><span data-stu-id="568de-426">For more information, see each framework's documentation.</span></span>

<span data-ttu-id="568de-427">È anche possibile creare provider personalizzati, per supportare altri framework di registrazione o requisiti di registrazione specifici.</span><span class="sxs-lookup"><span data-stu-id="568de-427">You can create your own custom providers as well, to support other logging frameworks or your own logging requirements.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="568de-428">Flusso di registrazione di Azure</span><span class="sxs-lookup"><span data-stu-id="568de-428">Azure log streaming</span></span>

<span data-ttu-id="568de-429">Il flusso di registrazione di Azure consente di visualizzare l'attività di registrazione in tempo reale da:</span><span class="sxs-lookup"><span data-stu-id="568de-429">Azure log streaming enables you to view log activity in real time from:</span></span> 

* <span data-ttu-id="568de-430">Server applicazioni</span><span class="sxs-lookup"><span data-stu-id="568de-430">The application server</span></span> 
* <span data-ttu-id="568de-431">Server Web</span><span class="sxs-lookup"><span data-stu-id="568de-431">The web server</span></span>
* <span data-ttu-id="568de-432">Traccia delle richieste non riuscite</span><span class="sxs-lookup"><span data-stu-id="568de-432">Failed request tracing</span></span> 

<span data-ttu-id="568de-433">Per configurare il flusso di registrazione di Azure:</span><span class="sxs-lookup"><span data-stu-id="568de-433">To configure Azure log streaming:</span></span> 

* <span data-ttu-id="568de-434">Passare alla pagina **Log di diagnostica** dalla pagina del portale dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="568de-434">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="568de-435">Attivare **Registrazione applicazioni (file system)**.</span><span class="sxs-lookup"><span data-stu-id="568de-435">Set **Application Logging (Filesystem)** to on.</span></span> 

![Pagina dei log di diagnostica del portale di Azure](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="568de-437">Passare alla pagina **Flusso di registrazione** per visualizzare i messaggi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="568de-437">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="568de-438">Vengono registrati dall'applicazione tramite l'interfaccia `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="568de-438">They are logged by application through the `ILogger` interface.</span></span> 

![Flusso di registrazione dell'applicazione nel portale di Azure](index/_static/azure-log-streaming.png)


## <a name="see-also"></a><span data-ttu-id="568de-440">Vedere anche</span><span class="sxs-lookup"><span data-stu-id="568de-440">See also</span></span>

[<span data-ttu-id="568de-441">Registrazione ad alte prestazioni con LoggerMessage</span><span class="sxs-lookup"><span data-stu-id="568de-441">High-performance logging with LoggerMessage</span></span>](xref:fundamentals/logging/loggermessage)