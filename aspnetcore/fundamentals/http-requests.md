---
title: Inizializzare richieste HTTP
author: stevejgordon
description: Informazioni sull'uso dell'interfaccia IHttpClientFactory per gestire le istanze di HttpClient logiche in ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 05/02/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/http-requests
ms.openlocfilehash: 30ac239a38376feecffc3010387ec5e0009b6db6
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2018
---
# <a name="initiate-http-requests"></a><span data-ttu-id="25d32-103">Inizializzare richieste HTTP</span><span class="sxs-lookup"><span data-stu-id="25d32-103">Initiate HTTP requests</span></span>

<span data-ttu-id="25d32-104">Di [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) e [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="25d32-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

[!INCLUDE[](~/includes/2.1.md)]

<span data-ttu-id="25d32-105">È possibile registrare e usare `IHttpClientFactory` per configurare e creare istanze di [HttpClient](/dotnet/api/system.net.http.httpclient) in un'app.</span><span class="sxs-lookup"><span data-stu-id="25d32-105">An `IHttpClientFactory` can be registered and used to configure and create [HttpClient](/dotnet/api/system.net.http.httpclient) instances in an app.</span></span> <span data-ttu-id="25d32-106">I vantaggi offerti sono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="25d32-106">It offers the following benefits:</span></span>

* <span data-ttu-id="25d32-107">Offre una posizione centrale per la denominazione e la configurazione di istanze di `HttpClient` logiche.</span><span class="sxs-lookup"><span data-stu-id="25d32-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="25d32-108">Ad esempio, è possibile registrare e configurare un client "github" per accedere a GitHub.</span><span class="sxs-lookup"><span data-stu-id="25d32-108">For example, a "github" client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="25d32-109">È possibile registrare un client predefinito per altri scopi.</span><span class="sxs-lookup"><span data-stu-id="25d32-109">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="25d32-110">Codifica il concetto di middleware in uscita tramite la delega di gestori in `HttpClient` e offre estensioni per il middleware basato su Polly per sfruttarne i vantaggi.</span><span class="sxs-lookup"><span data-stu-id="25d32-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="25d32-111">Gestisce il pooling e la durata delle istanze di `HttpClientMessageHandler` sottostanti per evitare problemi DNS comuni che si verificano quando le durate di `HttpClient` vengono gestite manualmente.</span><span class="sxs-lookup"><span data-stu-id="25d32-111">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="25d32-112">Aggiunge un'esperienza di registrazione configurabile, tramite `ILogger`, per tutte le richieste inviate attraverso i client creati dalla factory.</span><span class="sxs-lookup"><span data-stu-id="25d32-112">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="25d32-113">Modelli di consumo</span><span class="sxs-lookup"><span data-stu-id="25d32-113">Consumption patterns</span></span>

<span data-ttu-id="25d32-114">`IHttpClientFactory` può essere usato in un'app in diversi modi:</span><span class="sxs-lookup"><span data-stu-id="25d32-114">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="25d32-115">Utilizzo di base</span><span class="sxs-lookup"><span data-stu-id="25d32-115">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="25d32-116">Client denominati</span><span class="sxs-lookup"><span data-stu-id="25d32-116">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="25d32-117">Client tipizzati</span><span class="sxs-lookup"><span data-stu-id="25d32-117">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="25d32-118">Client generati</span><span class="sxs-lookup"><span data-stu-id="25d32-118">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="25d32-119">Nessuno di questi modi può essere considerato superiore a un altro.</span><span class="sxs-lookup"><span data-stu-id="25d32-119">None of them are strictly superior to another.</span></span> <span data-ttu-id="25d32-120">L'approccio migliore dipende dai vincoli dell'app.</span><span class="sxs-lookup"><span data-stu-id="25d32-120">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="25d32-121">Utilizzo di base</span><span class="sxs-lookup"><span data-stu-id="25d32-121">Basic usage</span></span>

<span data-ttu-id="25d32-122">È possibile registrare `IHttpClientFactory` chiamando il metodo di estensione `AddHttpClient` in `IServiceCollection`, all'interno del metodo `ConfigureServices` in Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="25d32-122">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `ConfigureServices` method in Startup.cs.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet1)]

<span data-ttu-id="25d32-123">Dopo la registrazione, il codice può accettare un'interfaccia `IHttpClientFactory` ovunque sia possibile inserire servizi con l'[inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="25d32-123">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="25d32-124">`IHttpClientFactory` può essere usato per creare un'istanza di `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="25d32-124">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,20)]

<span data-ttu-id="25d32-125">Questo uso di `IHttpClientFactory` è un ottimo modo per effettuare il refactoring di un'app esistente.</span><span class="sxs-lookup"><span data-stu-id="25d32-125">Using `IHttpClientFactory` in this fashion is a great way to refactor an existing app.</span></span> <span data-ttu-id="25d32-126">Non influisce in alcun modo sulle modalità d'uso di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="25d32-126">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="25d32-127">Nelle posizioni in cui vengono attualmente create istanze di `HttpClient`, sostituire le occorrenze con una chiamata a `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="25d32-127">In places where `HttpClient` instances are currently created, replace those occurrences with a call to `CreateClient`.</span></span>

### <a name="named-clients"></a><span data-ttu-id="25d32-128">Client denominati</span><span class="sxs-lookup"><span data-stu-id="25d32-128">Named clients</span></span>

<span data-ttu-id="25d32-129">Se un'app richiede più usi distinti di `HttpClient`, ognuno con una configurazione diversa, un'opzione è l'uso di **client denominati**.</span><span class="sxs-lookup"><span data-stu-id="25d32-129">If an app requires multiple distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="25d32-130">La configurazione di un `HttpClient` denominato può essere specificata durante la registrazione in `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="25d32-130">Configuration for a named `HttpClient` can be specified during registration in `ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet2)]

<span data-ttu-id="25d32-131">Nel codice precedente viene chiamato `AddHttpClient`, in cui viene specificato il nome "github".</span><span class="sxs-lookup"><span data-stu-id="25d32-131">In the preceding code, `AddHttpClient` is called, providing the name "github".</span></span> <span data-ttu-id="25d32-132">Al client viene applicata una configurazione predefinita, ovvero l'indirizzo di base e due intestazioni necessari per l'uso dell'API GitHub.</span><span class="sxs-lookup"><span data-stu-id="25d32-132">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="25d32-133">Ogni volta che `CreateClient` viene chiamato, verrà creata una nuova istanza di `HttpClient` e verrà chiamata l'azione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="25d32-133">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="25d32-134">Per usare un client denominato, è possibile passare un parametro di stringa a `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="25d32-134">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="25d32-135">Specificare il nome del client da creare:</span><span class="sxs-lookup"><span data-stu-id="25d32-135">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=20)]

<span data-ttu-id="25d32-136">Nel codice precedente non è necessario che la richiesta specifichi un nome host.</span><span class="sxs-lookup"><span data-stu-id="25d32-136">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="25d32-137">Poiché viene usato l'indirizzo di base configurato per il client, è possibile passare solo il percorso.</span><span class="sxs-lookup"><span data-stu-id="25d32-137">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="25d32-138">Client tipizzati</span><span class="sxs-lookup"><span data-stu-id="25d32-138">Typed clients</span></span>

<span data-ttu-id="25d32-139">I client tipizzati offrono le stesse funzionalità dei client denominati senza la necessità di usare le stringhe come chiavi.</span><span class="sxs-lookup"><span data-stu-id="25d32-139">Typed clients provide the same capabilities as named clients without the need to use strings as keys.</span></span> <span data-ttu-id="25d32-140">L'approccio basato su client tipizzati offre l'aiuto di IntelliSense e del compilatore quando si usano i client.</span><span class="sxs-lookup"><span data-stu-id="25d32-140">The typed client approach provides IntelliSense and compiler help when consuming clients.</span></span> <span data-ttu-id="25d32-141">La configurazione e l'interazione con un particolare `HttpClient` può avvenire in un'unica posizione.</span><span class="sxs-lookup"><span data-stu-id="25d32-141">They provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="25d32-142">Per un singolo endpoint back-end è ad esempio possibile usare un unico client tipizzato che incapsuli tutta la logica relativa all'endpoint.</span><span class="sxs-lookup"><span data-stu-id="25d32-142">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span> <span data-ttu-id="25d32-143">Un altro vantaggio è rappresentato dalla possibilità di usare l'inserimento di dipendenze e di inserirli dove necessario nell'app.</span><span class="sxs-lookup"><span data-stu-id="25d32-143">Another advantage is that they work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="25d32-144">Un client tipizzato accetta il parametro `HttpClient` nel proprio costruttore:</span><span class="sxs-lookup"><span data-stu-id="25d32-144">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="25d32-145">Nel codice precedente la configurazione viene spostata nel client tipizzato.</span><span class="sxs-lookup"><span data-stu-id="25d32-145">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="25d32-146">L'oggetto `HttpClient` viene esposto come una proprietà pubblica.</span><span class="sxs-lookup"><span data-stu-id="25d32-146">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="25d32-147">È possibile definire metodi di API specifiche che espongono la funzionalità `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="25d32-147">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="25d32-148">Il metodo `GetLatestDocsIssue` incapsula il codice necessario per eseguire una query e individuare il problema più recente pubblicato in un repository GitHub.</span><span class="sxs-lookup"><span data-stu-id="25d32-148">The `GetLatestDocsIssue` method encapsulates the code needed to query for and parse out the latest issue from a GitHub repository.</span></span>

<span data-ttu-id="25d32-149">Per registrare un client tipizzato è possibile usare il metodo di estensione `AddHttpClient` generico all'interno di `ConfigureServices`, specificando la classe del client tipizzato:</span><span class="sxs-lookup"><span data-stu-id="25d32-149">To register a typed client, the generic `AddHttpClient` extension method can be used within `ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet3)]

<span data-ttu-id="25d32-150">Il client tipizzato viene registrato come temporaneo nell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="25d32-150">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="25d32-151">Il client tipizzato può essere inserito e usato direttamente:</span><span class="sxs-lookup"><span data-stu-id="25d32-151">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="25d32-152">Se si preferisce, è possibile specificare la configurazione di un client tipizzato durante la registrazione in `ConfigureServices`, anziché nel relativo costruttore:</span><span class="sxs-lookup"><span data-stu-id="25d32-152">If preferred, the configuration for a typed client can be specified during registration in `ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet4)]

<span data-ttu-id="25d32-153">È possibile incapsulare interamente `HttpClient` all'interno di un client tipizzato.</span><span class="sxs-lookup"><span data-stu-id="25d32-153">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="25d32-154">Anziché esporlo come una proprietà, è possibile specificare metodi pubblici che chiamano l'istanza di `HttpClient` internamente.</span><span class="sxs-lookup"><span data-stu-id="25d32-154">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/GitHub/RepoService.cs?name=snippet1&highlight=3)]

<span data-ttu-id="25d32-155">Nel codice precedente `HttpClient` viene archiviato come un campo privato.</span><span class="sxs-lookup"><span data-stu-id="25d32-155">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="25d32-156">L'accesso per effettuare chiamate esterne passa attraverso il metodo `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="25d32-156">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="25d32-157">Client generati</span><span class="sxs-lookup"><span data-stu-id="25d32-157">Generated clients</span></span>

<span data-ttu-id="25d32-158">È possibile usare `IHttpClientFactory` in combinazione con altre librerie di terze parti, ad esempio [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="25d32-158">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="25d32-159">Refit è una libreria REST per .NET.</span><span class="sxs-lookup"><span data-stu-id="25d32-159">Refit is a REST library for .NET.</span></span> <span data-ttu-id="25d32-160">Converte le API REST in interfacce live.</span><span class="sxs-lookup"><span data-stu-id="25d32-160">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="25d32-161">Un'implementazione dell'interfaccia viene generata dinamicamente da `RestService`, usando `HttpClient` per effettuare le chiamate HTTP esterne.</span><span class="sxs-lookup"><span data-stu-id="25d32-161">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="25d32-162">Per rappresentare l'API esterna e la relativa risposta vengono definite un'interfaccia e una risposta:</span><span class="sxs-lookup"><span data-stu-id="25d32-162">An interface and a reply are defined to represent the external API and its response:</span></span>

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

<span data-ttu-id="25d32-163">È possibile aggiungere un client tipizzato usando Refit per generare l'implementazione:</span><span class="sxs-lookup"><span data-stu-id="25d32-163">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="25d32-164">L'interfaccia definita può essere usata dove necessario, con l'implementazione offerta dall'inserimento di dipendenze e da Refit:</span><span class="sxs-lookup"><span data-stu-id="25d32-164">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a><span data-ttu-id="25d32-165">Middleware per richieste in uscita</span><span class="sxs-lookup"><span data-stu-id="25d32-165">Outgoing request middleware</span></span>

<span data-ttu-id="25d32-166">`HttpClient` include già il concetto di delega di gestori concatenati per le richieste HTTP in uscita.</span><span class="sxs-lookup"><span data-stu-id="25d32-166">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="25d32-167">`IHttpClientFactory` semplifica la definizione dei gestori da applicare per ogni client denominato.</span><span class="sxs-lookup"><span data-stu-id="25d32-167">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="25d32-168">Supporta la registrazione e il concatenamento di più gestori per creare una pipeline di middleware per le richieste in uscita.</span><span class="sxs-lookup"><span data-stu-id="25d32-168">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="25d32-169">Ognuno di questi gestori è in grado di eseguire operazioni prima e dopo la richiesta in uscita.</span><span class="sxs-lookup"><span data-stu-id="25d32-169">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="25d32-170">Questo modello è simile alla pipeline di middleware in ingresso in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="25d32-170">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="25d32-171">Il modello offre un meccanismo per gestire le problematiche trasversali relative alle richieste HTTP, tra cui memorizzazione nella cache, gestione degli errori, serializzazione e registrazione.</span><span class="sxs-lookup"><span data-stu-id="25d32-171">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="25d32-172">Per creare un gestore, definire una classe che deriva da `DelegatingHandler`.</span><span class="sxs-lookup"><span data-stu-id="25d32-172">To create a handler, define a class deriving from `DelegatingHandler`.</span></span> <span data-ttu-id="25d32-173">Eseguire l'override del metodo `SendAsync` per eseguire il codice prima di passare la richiesta al gestore successivo nella pipeline:</span><span class="sxs-lookup"><span data-stu-id="25d32-173">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[Main](http-requests/samples/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="25d32-174">Il codice precedente definisce un gestore di base.</span><span class="sxs-lookup"><span data-stu-id="25d32-174">The preceding code defines a basic handler.</span></span> <span data-ttu-id="25d32-175">Verifica se un'intestazione X-API-KEY è stata inclusa nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="25d32-175">It checks to see if an X-API-KEY header has been included on the request.</span></span> <span data-ttu-id="25d32-176">Se l'intestazione non è presente, può evitare la chiamata HTTP e restituire una risposta appropriata.</span><span class="sxs-lookup"><span data-stu-id="25d32-176">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="25d32-177">Durante la registrazione è possibile aggiungere alla configurazione uno o più gestori per un'istanza di `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="25d32-177">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="25d32-178">Questa attività viene eseguita tramite metodi di estensione in `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="25d32-178">This task is accomplished via extension methods on the `IHttpClientBuilder`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet5)]

<span data-ttu-id="25d32-179">Nel codice precedente `ValidateHeaderHandler` viene registrato nell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="25d32-179">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="25d32-180">Il gestore **deve** essere registrato come temporaneo nell'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="25d32-180">The handler **must** be registered in DI as transient.</span></span> <span data-ttu-id="25d32-181">Dopo la registrazione è possibile chiamare `AddHttpMessageHandler`, passando il tipo di gestore.</span><span class="sxs-lookup"><span data-stu-id="25d32-181">Once registered, `AddHttpMessageHandler` can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="25d32-182">È possibile registrare più gestori nell'ordine di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="25d32-182">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="25d32-183">Ogni gestore esegue il wrapping del gestore successivo fino a quando l'elemento `HttpClientHandler` finale non esegue la richiesta:</span><span class="sxs-lookup"><span data-stu-id="25d32-183">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet6)]

## <a name="use-polly-based-handlers"></a><span data-ttu-id="25d32-184">Usare gestori basati su Polly</span><span class="sxs-lookup"><span data-stu-id="25d32-184">Use Polly-based handlers</span></span>

<span data-ttu-id="25d32-185">`IHttpClientFactory` si integra con una libreria di terze parti piuttosto diffusa denominata [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="25d32-185">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="25d32-186">Polly è una libreria di gestione degli errori temporanei e di resilienza completa per .NET.</span><span class="sxs-lookup"><span data-stu-id="25d32-186">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="25d32-187">Consente agli sviluppatori di esprimere criteri quali Retry, Circuit Breaker, Timeout, Bulkhead Isolation e Fallback in modo fluido e thread-safe.</span><span class="sxs-lookup"><span data-stu-id="25d32-187">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="25d32-188">Per consentire l'uso dei criteri Polly con le istanze configurate di `HttpClient` sono disponibili metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="25d32-188">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="25d32-189">Le estensioni per Polly sono disponibili in un pacchetto NuGet denominato "Microsoft.Extensions.Http.Polly".</span><span class="sxs-lookup"><span data-stu-id="25d32-189">The Polly extensions are available in a NuGet package called 'Microsoft.Extensions.Http.Polly'.</span></span> <span data-ttu-id="25d32-190">Questo pacchetto non è incluso per impostazione predefinita dal metapacchetto "Microsoft.AspNetCore.App".</span><span class="sxs-lookup"><span data-stu-id="25d32-190">This package is not included by default by the 'Microsoft.AspNetCore.App' metapackage.</span></span> <span data-ttu-id="25d32-191">Per usare le estensioni è necessario includere esplicitamente un elemento PackageReference nel progetto.</span><span class="sxs-lookup"><span data-stu-id="25d32-191">To use the extensions, a PackageReference should be explicitly included in the project.</span></span>

[!code-csharp[](http-requests/samples/HttpClientFactorySample.csproj?highlight=9)]

<span data-ttu-id="25d32-192">Dopo il ripristino di questo pacchetto, i metodi di estensione che supportano l'aggiunta di gestori basati su Polly ai client risultano disponibili.</span><span class="sxs-lookup"><span data-stu-id="25d32-192">After restoring this package, extension methods are available to support adding Polly-based handlers to clients.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="25d32-193">Gestire gli errori temporanei</span><span class="sxs-lookup"><span data-stu-id="25d32-193">Handle transient faults</span></span>

<span data-ttu-id="25d32-194">Gli errori più comuni che si prevede possano verificarsi quando si effettuano chiamate HTTP esterne saranno temporanei.</span><span class="sxs-lookup"><span data-stu-id="25d32-194">The most common faults you may expect to occur when making external HTTP calls will be transient.</span></span> <span data-ttu-id="25d32-195">Per definire un criterio in grado di gestire gli errori temporanei è disponibile un pratico metodo di estensione denominato `AddTransientHttpErrorPolicy`.</span><span class="sxs-lookup"><span data-stu-id="25d32-195">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="25d32-196">I criteri configurati con questo metodo di estensione gestiscono `HttpRequestException`, risposte HTTP 5xx e risposte HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="25d32-196">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="25d32-197">L'estensione `AddTransientHttpErrorPolicy` può essere usata all'interno di `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="25d32-197">The `AddTransientHttpErrorPolicy` extension can be used within `ConfigureServices`.</span></span> <span data-ttu-id="25d32-198">L'estensione consente l'accesso a un oggetto `PolicyBuilder` configurato per gestire gli errori che rappresentano un possibile errore temporaneo:</span><span class="sxs-lookup"><span data-stu-id="25d32-198">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet7)]

<span data-ttu-id="25d32-199">Nel codice precedente viene definito un criterio `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="25d32-199">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="25d32-200">Le richieste non riuscite vengono ritentate fino a tre volte con un ritardo di 600 millisecondi tra i tentativi.</span><span class="sxs-lookup"><span data-stu-id="25d32-200">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="25d32-201">Selezionare i criteri in modo dinamico</span><span class="sxs-lookup"><span data-stu-id="25d32-201">Dynamically select policies</span></span>

<span data-ttu-id="25d32-202">Per aggiungere gestori basati su Polly è possibile usare altri metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="25d32-202">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="25d32-203">Una di queste estensioni è `AddPolicyHandler`, che include più overload.</span><span class="sxs-lookup"><span data-stu-id="25d32-203">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="25d32-204">Un overload consente l'ispezione della richiesta al momento della definizione del criterio da applicare:</span><span class="sxs-lookup"><span data-stu-id="25d32-204">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet8)]

<span data-ttu-id="25d32-205">Nel codice precedente se la richiesta in uscita è un'operazione GET, viene applicato un timeout di 10 secondi.</span><span class="sxs-lookup"><span data-stu-id="25d32-205">In the preceding code, if the outgoing request is a GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="25d32-206">Per qualsiasi altro metodo HTTP viene usato un timeout di 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="25d32-206">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="25d32-207">Aggiungere più gestori Polly</span><span class="sxs-lookup"><span data-stu-id="25d32-207">Add multiple Polly handlers</span></span>

<span data-ttu-id="25d32-208">Una pratica comune per offrire una funzionalità avanzata è quella di annidare i criteri Polly:</span><span class="sxs-lookup"><span data-stu-id="25d32-208">It is common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet9)]

<span data-ttu-id="25d32-209">Nell'esempio precedente vengono aggiunti due gestori.</span><span class="sxs-lookup"><span data-stu-id="25d32-209">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="25d32-210">Il primo usa l'estensione `AddTransientHttpErrorPolicy` per aggiungere criteri di ripetizione.</span><span class="sxs-lookup"><span data-stu-id="25d32-210">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="25d32-211">Le richieste non riuscite vengono ritentate fino a tre volte.</span><span class="sxs-lookup"><span data-stu-id="25d32-211">Failed requests are retried up to three times.</span></span> <span data-ttu-id="25d32-212">La seconda chiamata a `AddTransientHttpErrorPolicy` aggiunge criteri dell'interruttore di circuito.</span><span class="sxs-lookup"><span data-stu-id="25d32-212">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="25d32-213">Ulteriori richieste esterne vengono bloccate per 30 secondi nel caso si verifichino cinque tentativi non riusciti consecutivi.</span><span class="sxs-lookup"><span data-stu-id="25d32-213">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="25d32-214">I criteri dell'interruttore di circuito sono con stato.</span><span class="sxs-lookup"><span data-stu-id="25d32-214">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="25d32-215">Tutte le chiamate tramite questo client condividono lo stesso stato di circuito.</span><span class="sxs-lookup"><span data-stu-id="25d32-215">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="25d32-216">Aggiungere criteri dal registro Polly</span><span class="sxs-lookup"><span data-stu-id="25d32-216">Add policies from the Polly registry</span></span>

<span data-ttu-id="25d32-217">Un approccio alla gestione dei criteri usati di frequente consiste nel definirli una volta e registrarli in un elemento `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="25d32-217">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="25d32-218">Per aggiungere un gestore usando un criterio del registro è disponibile un metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="25d32-218">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet10)]

<span data-ttu-id="25d32-219">Nel codice precedente viene aggiunto un elemento PolicyRegistry a `ServiceCollection` e vengono registrati due criteri.</span><span class="sxs-lookup"><span data-stu-id="25d32-219">In the preceding code, a PolicyRegistry is added to the `ServiceCollection` and two policies are registered with it.</span></span> <span data-ttu-id="25d32-220">Per usare un criterio del registro viene usato il metodo `AddPolicyHandlerFromRegistry` passando il nome del criterio da applicare.</span><span class="sxs-lookup"><span data-stu-id="25d32-220">In order to use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="25d32-221">Altre informazioni su `IHttpClientFactory` e le integrazioni con Polly sono disponibili nel [wiki di Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="25d32-221">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="25d32-222">Gestione di HttpClient e durata</span><span class="sxs-lookup"><span data-stu-id="25d32-222">HttpClient and lifetime management</span></span>

<span data-ttu-id="25d32-223">Ogni volta che `CreateClient` viene chiamato in `IHttpClientFactory` viene restituita una nuova istanza di un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="25d32-223">Each time `CreateClient` is called on the `IHttpClientFactory`, a new instance of a `HttpClient` is returned.</span></span> <span data-ttu-id="25d32-224">Per ogni client denominato sarà presente un elemento `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="25d32-224">There will be a `HttpMessageHandler` per named client.</span></span> <span data-ttu-id="25d32-225">`IHttpClientFactory` eseguirà il pooling delle istanze di `HttpMessageHandler` create dalla factory per ridurre il consumo di risorse.</span><span class="sxs-lookup"><span data-stu-id="25d32-225">`IHttpClientFactory` will pool the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="25d32-226">Un'istanza di `HttpMessageHandler` può essere riusata dal pool quando si crea una nuova istanza di `HttpClient` se la relativa durata non è scaduta.</span><span class="sxs-lookup"><span data-stu-id="25d32-226">A `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span> 

<span data-ttu-id="25d32-227">Il pooling dei gestori è opportuno poiché ogni gestore si occupa in genere di gestire le proprie connessioni HTTP sottostanti. La creazione di un numero superiore di gestori rispetto a quelli necessari può determinare ritardi di connessione.</span><span class="sxs-lookup"><span data-stu-id="25d32-227">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections; creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="25d32-228">Alcuni gestori mantengono inoltre le connessioni aperte a tempo indefinito. Ciò può impedire al gestore di reagire alle modifiche DNS.</span><span class="sxs-lookup"><span data-stu-id="25d32-228">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="25d32-229">La durata del gestore predefinito è di due minuti.</span><span class="sxs-lookup"><span data-stu-id="25d32-229">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="25d32-230">Il valore predefinito può essere sottoposto a override per ogni client denominato.</span><span class="sxs-lookup"><span data-stu-id="25d32-230">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="25d32-231">Per eseguire l'override, chiamare `SetHandlerLifetime` nell'elemento `IHttpClientBuilder` restituito al momento della creazione del client:</span><span class="sxs-lookup"><span data-stu-id="25d32-231">To override it, call `SetHandlerLifetime` on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet11)]

## <a name="logging"></a><span data-ttu-id="25d32-232">Registrazione</span><span class="sxs-lookup"><span data-stu-id="25d32-232">Logging</span></span>

<span data-ttu-id="25d32-233">I client creati tramite `IHttpClientFactory` registrano i messaggi di log per tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="25d32-233">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="25d32-234">Per visualizzare i messaggi di log predefiniti è necessario abilitare il livello di informazioni appropriato nella configurazione di registrazione.</span><span class="sxs-lookup"><span data-stu-id="25d32-234">You'll need to enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="25d32-235">La registrazione aggiuntiva, ad esempio quella delle intestazioni delle richieste, è inclusa solo a livello di traccia.</span><span class="sxs-lookup"><span data-stu-id="25d32-235">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="25d32-236">La categoria di log usata per ogni client include il nome del client.</span><span class="sxs-lookup"><span data-stu-id="25d32-236">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="25d32-237">Un client denominato "MyNamedClient", ad esempio, registra i messaggi con una categoria `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="25d32-237">A client named "MyNamedClient", for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="25d32-238">I messaggi con il suffisso "LogicalHandler" sono all'esterno della pipeline del gestore richieste.</span><span class="sxs-lookup"><span data-stu-id="25d32-238">Messages with the suffix of "LogicalHandler" occur on the outside of request handler pipeline.</span></span> <span data-ttu-id="25d32-239">Nella richiesta i messaggi vengono registrati prima che qualsiasi altro gestore nella pipeline l'abbia elaborata.</span><span class="sxs-lookup"><span data-stu-id="25d32-239">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="25d32-240">Nella risposta i messaggi vengono registrati dopo che qualsiasi altro gestore nella pipeline ha ricevuto la risposta.</span><span class="sxs-lookup"><span data-stu-id="25d32-240">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="25d32-241">La registrazione avviene anche all'interno della pipeline del gestore richieste.</span><span class="sxs-lookup"><span data-stu-id="25d32-241">Logging also occurs on the inside of the request handler pipeline.</span></span> <span data-ttu-id="25d32-242">Nel caso dell'esempio "MyNamedClient", i messaggi vengono registrati nella categoria di log `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="25d32-242">In the case of the "MyNamedClient" example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="25d32-243">Per la richiesta, ciò avviene dopo l'esecuzione di tutti gli altri gestori e immediatamente prima che la richiesta sia inviata in rete.</span><span class="sxs-lookup"><span data-stu-id="25d32-243">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="25d32-244">Nella risposta la registrazione include lo stato della risposta prima di restituirla attraverso la pipeline del gestore.</span><span class="sxs-lookup"><span data-stu-id="25d32-244">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="25d32-245">L'abilitazione della registrazione all'esterno e all'interno della pipeline consente l'ispezione delle modifiche apportate da altri gestori nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="25d32-245">Enabling logging on the outside and inside of the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="25d32-246">Le modifiche possono ad esempio riguardare le intestazioni delle richieste o il codice di stato della risposta.</span><span class="sxs-lookup"><span data-stu-id="25d32-246">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="25d32-247">L'inclusione del nome del client nella categoria di log consente di filtrare i log in base a client denominati specifici, se necessario.</span><span class="sxs-lookup"><span data-stu-id="25d32-247">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="25d32-248">Configurare HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="25d32-248">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="25d32-249">Può essere necessario controllare la configurazione dell'elemento `HttpMessageHandler` interno usato da un client.</span><span class="sxs-lookup"><span data-stu-id="25d32-249">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="25d32-250">Quando si aggiungono client denominati o tipizzati viene restituito un elemento `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="25d32-250">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="25d32-251">È possibile usare il metodo di estensione `ConfigurePrimaryHttpMessageHandler` per definire un delegato.</span><span class="sxs-lookup"><span data-stu-id="25d32-251">The `ConfigurePrimaryHttpMessageHandler` extension method can be used to define a delegate.</span></span> <span data-ttu-id="25d32-252">Il delegato viene usato per creare e configurare l'elemento `HttpMessageHandler` primario usato dal client:</span><span class="sxs-lookup"><span data-stu-id="25d32-252">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet12)]