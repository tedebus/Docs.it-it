---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
title: Esaminare i dettagli e i metodi di eliminazione | Documenti Microsoft
author: Rick-Anderson
description: "Nota: Una versione aggiornata di questa esercitazione è disponibile qui che utilizza ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice seguire e demo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 11425ff3-09fc-4efa-be9a-b53bce503460
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 213626147424e08d10d6442034ec450174200b09
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="examining-the-details-and-delete-methods"></a><span data-ttu-id="dc537-104">Esaminare i dettagli e i metodi di eliminazione</span><span class="sxs-lookup"><span data-stu-id="dc537-104">Examining the Details and Delete Methods</span></span>
====================
<span data-ttu-id="dc537-105">Da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="dc537-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="dc537-106">È disponibile una versione aggiornata di questa esercitazione [qui](../../getting-started/introduction/getting-started.md) che utilizza ASP.NET MVC 5 e Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="dc537-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="dc537-107">È molto più semplice da seguire, più sicuro e vengono illustrate altre funzionalità.</span><span class="sxs-lookup"><span data-stu-id="dc537-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="dc537-108">In questa parte dell'esercitazione, si esamineranno generato automaticamente `Details` e `Delete` metodi.</span><span class="sxs-lookup"><span data-stu-id="dc537-108">In this part of the tutorial, you'll examine the automatically generated `Details` and `Delete` methods.</span></span>

## <a name="examining-the-details-and-delete-methods"></a><span data-ttu-id="dc537-109">Esaminare i dettagli e i metodi di eliminazione</span><span class="sxs-lookup"><span data-stu-id="dc537-109">Examining the Details and Delete Methods</span></span>

<span data-ttu-id="dc537-110">Aprire il `Movie` controller ed esaminare il `Details` metodo.</span><span class="sxs-lookup"><span data-stu-id="dc537-110">Open the `Movie` controller and examine the `Details` method.</span></span>

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

<span data-ttu-id="dc537-111">Il motore di scaffolding MVC che ha creato questo metodo di azione aggiunge un commento che mostra una richiesta HTTP che richiama il metodo.</span><span class="sxs-lookup"><span data-stu-id="dc537-111">The MVC scaffolding engine that created this action method adds a comment showing a HTTP request that invokes the method.</span></span> <span data-ttu-id="dc537-112">In questo caso è un `GET` richiesta con tre segmenti di URL, il `Movies` controller, il `Details` (metodo) e un `ID` valore.</span><span class="sxs-lookup"><span data-stu-id="dc537-112">In this case it's a `GET` request with three URL segments, the `Movies` controller, the `Details` method and a `ID` value.</span></span>

<span data-ttu-id="dc537-113">Codice innanzitutto semplifica la ricerca di dati utilizzando il `Find` metodo.</span><span class="sxs-lookup"><span data-stu-id="dc537-113">Code First makes it easy to search for data using the `Find` method.</span></span> <span data-ttu-id="dc537-114">Un'importante funzionalità di protezione creata nel metodo è che il codice verifica che il `Find` metodo ha trovato un filmato prima che il codice tenta di utilizzarlo.</span><span class="sxs-lookup"><span data-stu-id="dc537-114">An important security feature built into the method is that the code verifies that the `Find` method has found a movie before the code tries to do anything with it.</span></span> <span data-ttu-id="dc537-115">Ad esempio, un pirata informatico potrebbe causare errori nel sito modificando l'URL creato per i collegamenti da `http://localhost:xxxx/Movies/Details/1` esempio `http://localhost:xxxx/Movies/Details/12345` (o altri valori che non rappresentano un filmato effettivo).</span><span class="sxs-lookup"><span data-stu-id="dc537-115">For example, a hacker could introduce errors into the site by changing the URL created by the links from `http://localhost:xxxx/Movies/Details/1` to something like `http://localhost:xxxx/Movies/Details/12345` (or some other value that doesn't represent an actual movie).</span></span> <span data-ttu-id="dc537-116">Se non è stata selezionata per un filmato null, un film null restituirà un errore di database.</span><span class="sxs-lookup"><span data-stu-id="dc537-116">If you did not check for a null movie, a null movie would result in a database error.</span></span>

<span data-ttu-id="dc537-117">Esaminare i metodi `Delete` e `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="dc537-117">Examine the `Delete` and `DeleteConfirmed` methods.</span></span>

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

<span data-ttu-id="dc537-118">Si noti che il `HTTP Get``Delete` metodo non comporta l'eliminazione del filmato specificato, restituisce una visualizzazione del film in cui è possibile inviare (`HttpPost`) l'eliminazione...</span><span class="sxs-lookup"><span data-stu-id="dc537-118">Note that the `HTTP Get``Delete` method doesn't delete the specified movie, it returns a view of the movie where you can submit (`HttpPost`) the deletion..</span></span> <span data-ttu-id="dc537-119">L'esecuzione di un'operazione di eliminazione in risposta a una richiesta GET (o l'esecuzione di un'operazione di modifica, di creazione o di qualsiasi altra azione che modifica i dati) introduce un problema di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="dc537-119">Performing a delete operation in response to a GET request (or for that matter, performing an edit operation, create operation, or any other operation that changes data) opens up a security hole.</span></span> <span data-ttu-id="dc537-120">Per ulteriori informazioni, vedere il post di blog di Stephen Walther [ASP.NET MVC suggerimento #46-non usare eliminare collegamenti poiché creano problemi di sicurezza](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).</span><span class="sxs-lookup"><span data-stu-id="dc537-120">For more information about this, see Stephen Walther's blog entry [ASP.NET MVC Tip #46 — Don't use Delete Links because they create Security Holes](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).</span></span>

<span data-ttu-id="dc537-121">Il metodo `HttpPost` che elimina i dati è denominato `DeleteConfirmed` per fornire al metodo HTTP POST un nome o una firma univoca.</span><span class="sxs-lookup"><span data-stu-id="dc537-121">The `HttpPost` method that deletes the data is named `DeleteConfirmed` to give the HTTP POST method a unique signature or name.</span></span> <span data-ttu-id="dc537-122">Le firme dei due metodi sono illustrate di seguito:</span><span class="sxs-lookup"><span data-stu-id="dc537-122">The two method signatures are shown below:</span></span>

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

<span data-ttu-id="dc537-123">Il Common Language Runtime (CLR) richiede metodi in rapporto di overload per disporre di una firma di parametro univoca, ovvero lo stesso nome di metodo ma un elenco diverso di parametri.</span><span class="sxs-lookup"><span data-stu-id="dc537-123">The common language runtime (CLR) requires overloaded methods to have a unique parameter signature (same method name but different list of parameters).</span></span> <span data-ttu-id="dc537-124">Tuttavia, in questo caso è necessario due metodi di eliminazione: uno per GET - e uno per POST che entrambi abbiano la stessa firma del parametro.</span><span class="sxs-lookup"><span data-stu-id="dc537-124">However, here you need two Delete methods -- one for GET and one for POST -- that both have the same parameter signature.</span></span> <span data-ttu-id="dc537-125">Entrambi devono accettare un singolo intero come parametro.</span><span class="sxs-lookup"><span data-stu-id="dc537-125">(They both need to accept a single integer as a parameter.)</span></span>

<span data-ttu-id="dc537-126">Per ordinare la natura, è possibile eseguire alcune operazioni.</span><span class="sxs-lookup"><span data-stu-id="dc537-126">To sort this out, you can do a couple of things.</span></span> <span data-ttu-id="dc537-127">Uno consiste nell'assegnare i metodi di nomi diversi.</span><span class="sxs-lookup"><span data-stu-id="dc537-127">One is to give the methods different names.</span></span> <span data-ttu-id="dc537-128">Questa operazione è stata eseguita dal meccanismo di scaffolding nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="dc537-128">That's what the scaffolding mechanism did in the preceding example.</span></span> <span data-ttu-id="dc537-129">Tuttavia in questo modo si introduce un piccolo problema: ASP.NET esegue il mapping dei segmenti di un URL ai metodi di azione in base al nome e, se si rinomina un metodo, generalmente il routing non è in grado di trovare questo metodo.</span><span class="sxs-lookup"><span data-stu-id="dc537-129">However, this introduces a small problem: ASP.NET maps segments of a URL to action methods by name, and if you rename a method, routing normally wouldn't be able to find that method.</span></span> <span data-ttu-id="dc537-130">La soluzione è mostrata nell'esempio e consiste nell'aggiungere l'attributo `ActionName("Delete")` al metodo `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="dc537-130">The solution is what you see in the example, which is to add the `ActionName("Delete")` attribute to the `DeleteConfirmed` method.</span></span> <span data-ttu-id="dc537-131">Questo in modo efficace esegue il mapping per il sistema di routing in modo che un URL che includa */Delete/*disponibili per un POST di richiesta di `DeleteConfirmed` metodo.</span><span class="sxs-lookup"><span data-stu-id="dc537-131">This effectively performs mapping for the routing system so that a URL that includes */Delete/*for a POST request will find the `DeleteConfirmed` method.</span></span>

<span data-ttu-id="dc537-132">Un altro modo comune per evitare un problema con metodi che hanno firme e i nomi identici consiste nel modificare artificialmente la firma del metodo POST per includere un parametro non utilizzato.</span><span class="sxs-lookup"><span data-stu-id="dc537-132">Another common way to avoid a problem with methods that have identical names and signatures is to artificially change the signature of the POST method to include an unused parameter.</span></span> <span data-ttu-id="dc537-133">Ad esempio, alcuni sviluppatori aggiungono un parametro di tipo `FormCollection` che viene passato al metodo POST e quindi semplicemente non usa il parametro:</span><span class="sxs-lookup"><span data-stu-id="dc537-133">For example, some developers add a parameter type `FormCollection` that is passed to the POST method, and then simply don't use the parameter:</span></span>

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a><span data-ttu-id="dc537-134">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="dc537-134">Summary</span></span>

<span data-ttu-id="dc537-135">Ora è un'applicazione ASP.NET MVC completa che archivia i dati in un database di database locale.</span><span class="sxs-lookup"><span data-stu-id="dc537-135">You now have a complete ASP.NET MVC application that stores data in a local DB database.</span></span> <span data-ttu-id="dc537-136">È possibile creare, leggere, aggiornare, eliminare e cercare film.</span><span class="sxs-lookup"><span data-stu-id="dc537-136">You can create, read, update, delete, and search for movies.</span></span>

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a><span data-ttu-id="dc537-137">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="dc537-137">Next Steps</span></span>

<span data-ttu-id="dc537-138">Dopo aver compilato e testato un'applicazione web, il passaggio successivo è per renderlo disponibile ad altri utenti per l'utilizzo su Internet.</span><span class="sxs-lookup"><span data-stu-id="dc537-138">After you have built and tested a web application, the next step is to make it available to other people to use over the Internet.</span></span> <span data-ttu-id="dc537-139">A tale scopo, è necessario distribuirla in un provider di hosting web.</span><span class="sxs-lookup"><span data-stu-id="dc537-139">To do that, you have to deploy it to a web hosting provider.</span></span> <span data-ttu-id="dc537-140">Microsoft offre l'hosting web gratuito per fino a 10 siti web in un [senza account di valutazione di Windows Azure](https://www.windowsazure.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="dc537-140">Microsoft offers free web hosting for up to 10 web sites in a [free Windows Azure trial account](https://www.windowsazure.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="dc537-141">È consigliabile quindi seguire l'esercitazione [distribuire un'app di ASP.NET MVC Secure con appartenenza, OAuth e il Database SQL a un sito Web di Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="dc537-141">I suggest you next follow my tutorial [Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to a Windows Azure Web Site](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="dc537-142">Un'ottima esercitazione è di livello intermedio di Tom Dykstra [la creazione di un modello di dati di Entity Framework per un'applicazione MVC ASP.NET](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="dc537-142">An excellent tutorial is Tom Dykstra's intermediate-level [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="dc537-143">[StackOverflow](http://stackoverflow.com/help) e [forum di ASP.NET MVC](https://forums.asp.net/1146.aspx) sono inserisce un'eccellente per porre domande.</span><span class="sxs-lookup"><span data-stu-id="dc537-143">[Stackoverflow](http://stackoverflow.com/help) and the [ASP.NET MVC forums](https://forums.asp.net/1146.aspx) are a great places to ask questions.</span></span> <span data-ttu-id="dc537-144">Seguire [me](https://twitter.com/RickAndMSFT) su twitter, pertanto è possibile ottenere gli aggiornamenti in my esercitazioni più recente.</span><span class="sxs-lookup"><span data-stu-id="dc537-144">Follow [me](https://twitter.com/RickAndMSFT) on twitter so you can get updates on my latest tutorials.</span></span>

<span data-ttu-id="dc537-145">Commenti e suggerimenti sono ben accetti.</span><span class="sxs-lookup"><span data-stu-id="dc537-145">Feedback is welcome.</span></span>

<span data-ttu-id="dc537-146">- [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter:[@RickAndMSFT](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dc537-146">— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)</span></span>  
<span data-ttu-id="dc537-147">- [Scott Hanselman](http://www.hanselman.com/blog/) twitter:[@shanselman](https://twitter.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="dc537-147">— [Scott Hanselman](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="dc537-148">Precedente</span><span class="sxs-lookup"><span data-stu-id="dc537-148">Previous</span></span>](adding-validation-to-the-model.md)