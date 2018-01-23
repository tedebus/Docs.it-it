---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Creazione di un vincolo di Route personalizzati (VB) | Documenti Microsoft
author: StephenWalther
description: Stephen Walther viene illustrato come creare un vincolo di route personalizzati. Implementare una semplice personalizzato vincolo che impedisce l'elaborazione di una route corrispondente w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 41b04c1fea267e7ee9f8a0b1c2f0d4fe4bb96d15
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-custom-route-constraint-vb"></a><span data-ttu-id="b4148-104">Creazione di un vincolo di Route personalizzati (VB)</span><span class="sxs-lookup"><span data-stu-id="b4148-104">Creating a Custom Route Constraint (VB)</span></span>
====================
<span data-ttu-id="b4148-105">da [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="b4148-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="b4148-106">Stephen Walther viene illustrato come creare un vincolo di route personalizzati.</span><span class="sxs-lookup"><span data-stu-id="b4148-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="b4148-107">Viene implementato un vincolo personalizzato semplice che impedisce che una route viene cercata la corrispondenza quando viene effettuata una richiesta del browser da un computer remoto.</span><span class="sxs-lookup"><span data-stu-id="b4148-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="b4148-108">L'obiettivo di questa esercitazione è dimostrare come creare un vincolo di route personalizzati.</span><span class="sxs-lookup"><span data-stu-id="b4148-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="b4148-109">Un vincolo di route personalizzati consente di impedire che una route viene cercata la corrispondenza a meno che non esiste una corrispondenza per una condizione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="b4148-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="b4148-110">In questa esercitazione viene creato un vincolo di route Localhost.</span><span class="sxs-lookup"><span data-stu-id="b4148-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="b4148-111">Il vincolo di route Localhost corrisponde solo le richieste effettuate dal computer locale.</span><span class="sxs-lookup"><span data-stu-id="b4148-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="b4148-112">Richieste remote da su Internet non corrispondono.</span><span class="sxs-lookup"><span data-stu-id="b4148-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="b4148-113">Si implementa un vincolo di route personalizzati implementando l'interfaccia IRouteConstraint.</span><span class="sxs-lookup"><span data-stu-id="b4148-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="b4148-114">Questa è un'interfaccia molto semplice che descrive un singolo metodo:</span><span class="sxs-lookup"><span data-stu-id="b4148-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="b4148-115">Il metodo restituisce un valore booleano.</span><span class="sxs-lookup"><span data-stu-id="b4148-115">The method returns a Boolean value.</span></span> <span data-ttu-id="b4148-116">Se si restituisce False, la route associata con il vincolo non corrisponde alla richiesta di browser.</span><span class="sxs-lookup"><span data-stu-id="b4148-116">If you return False, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="b4148-117">Il vincolo Localhost è contenuto in elenco 1.</span><span class="sxs-lookup"><span data-stu-id="b4148-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="b4148-118">**Elenco 1 - LocalhostConstraint.vb**</span><span class="sxs-lookup"><span data-stu-id="b4148-118">**Listing 1 - LocalhostConstraint.vb**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="b4148-119">Il vincolo nel listato 1 consente di sfruttare la proprietà IsLocal esposta dalla classe HttpRequest.</span><span class="sxs-lookup"><span data-stu-id="b4148-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="b4148-120">Questa proprietà restituisce true quando l'indirizzo IP della richiesta è 127.0.0.1 o quando l'indirizzo IP della richiesta corrisponde all'indirizzo IP del server.</span><span class="sxs-lookup"><span data-stu-id="b4148-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="b4148-121">Utilizzare un vincolo personalizzato all'interno di una route definita nel file Global. asax.</span><span class="sxs-lookup"><span data-stu-id="b4148-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="b4148-122">Il file Global. asax listato 2 utilizza il vincolo Localhost per impedire a utenti di richiedere una pagina di amministrazione, a meno che non rendono la richiesta dal server locale.</span><span class="sxs-lookup"><span data-stu-id="b4148-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="b4148-123">Ad esempio, una richiesta per /Admin/DeleteAll avrà esito negativo quando effettuata da un server remoto.</span><span class="sxs-lookup"><span data-stu-id="b4148-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="b4148-124">**Elenco di 2 - Global. asax**</span><span class="sxs-lookup"><span data-stu-id="b4148-124">**Listing 2 - Global.asax**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="b4148-125">Il vincolo Localhost viene utilizzato nella definizione della route Admin.</span><span class="sxs-lookup"><span data-stu-id="b4148-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="b4148-126">Una richiesta del browser remoto non può corrispondere a questa route.</span><span class="sxs-lookup"><span data-stu-id="b4148-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="b4148-127">Si noti tuttavia che altre route definite in Global. asax potrebbero corrispondere la stessa richiesta.</span><span class="sxs-lookup"><span data-stu-id="b4148-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="b4148-128">È importante tenere presente che un vincolo impedisce una route specifica una richiesta di corrispondenza e non tutte le route definite nel file Global. asax.</span><span class="sxs-lookup"><span data-stu-id="b4148-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="b4148-129">Si noti che la route predefinita è stata impostata come commento nel file Global. asax listato 2.</span><span class="sxs-lookup"><span data-stu-id="b4148-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="b4148-130">Se si include la route predefinita, la route predefinita corrisponderebbe richieste per il controller di amministrazione.</span><span class="sxs-lookup"><span data-stu-id="b4148-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="b4148-131">In tal caso, gli utenti remoti può richiamare ancora azioni del controller di amministrazione, anche se le richieste non corrispondono alla route di amministrazione.</span><span class="sxs-lookup"><span data-stu-id="b4148-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="b4148-132">Precedente</span><span class="sxs-lookup"><span data-stu-id="b4148-132">Previous</span></span>](creating-a-route-constraint-vb.md)