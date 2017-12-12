---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Creazione di un'azione (VB) | Documenti Microsoft
author: microsoft
description: Informazioni su come aggiungere una nuova azione a un controller MVC ASP.NET. Informazioni sui requisiti per un metodo da un'azione.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: 3d1d355599c17df597f9c08d9d7f129fffc1a2e4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="creating-an-action-vb"></a><span data-ttu-id="fbfbe-104">Creazione di un'azione (VB)</span><span class="sxs-lookup"><span data-stu-id="fbfbe-104">Creating an Action (VB)</span></span>
====================
<span data-ttu-id="fbfbe-105">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="fbfbe-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="fbfbe-106">Informazioni su come aggiungere una nuova azione a un controller MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fbfbe-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="fbfbe-107">Informazioni sui requisiti per un metodo da un'azione.</span><span class="sxs-lookup"><span data-stu-id="fbfbe-107">Learn about the requirements for a method to be an action.</span></span>


<span data-ttu-id="fbfbe-108">L'obiettivo di questa esercitazione è illustrare come è possibile creare una nuova azione del controller.</span><span class="sxs-lookup"><span data-stu-id="fbfbe-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="fbfbe-109">Informazioni sui requisiti di un metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="fbfbe-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="fbfbe-110">Inoltre informazioni su come evitare che un metodo viene esposta come un'azione.</span><span class="sxs-lookup"><span data-stu-id="fbfbe-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="fbfbe-111">Aggiunta di un'azione a un Controller</span><span class="sxs-lookup"><span data-stu-id="fbfbe-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="fbfbe-112">Aggiungere una nuova azione a un controller aggiungendo un nuovo metodo al controller.</span><span class="sxs-lookup"><span data-stu-id="fbfbe-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="fbfbe-113">Ad esempio, il controller nel listato 1 contiene un'azione denominata index () e un'azione denominata sayHello ().</span><span class="sxs-lookup"><span data-stu-id="fbfbe-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="fbfbe-114">Entrambi i metodi vengono esposti come azioni.</span><span class="sxs-lookup"><span data-stu-id="fbfbe-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="fbfbe-115">**Elenco 1 - Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="fbfbe-115">**Listing 1 - Controllers\HomeController.vb**</span></span>

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

<span data-ttu-id="fbfbe-116">Per poter essere esposto all'universo come un'azione, un metodo deve soddisfare determinati requisiti:</span><span class="sxs-lookup"><span data-stu-id="fbfbe-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="fbfbe-117">Il metodo deve essere pubblico.</span><span class="sxs-lookup"><span data-stu-id="fbfbe-117">The method must be public.</span></span>
- <span data-ttu-id="fbfbe-118">Il metodo non può essere un metodo statico.</span><span class="sxs-lookup"><span data-stu-id="fbfbe-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="fbfbe-119">Il metodo non può essere un metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="fbfbe-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="fbfbe-120">Il metodo non può essere un costruttore, getter o setter.</span><span class="sxs-lookup"><span data-stu-id="fbfbe-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="fbfbe-121">Il metodo non può avere tipi generici aperti.</span><span class="sxs-lookup"><span data-stu-id="fbfbe-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="fbfbe-122">Il metodo non è un metodo della classe controller.</span><span class="sxs-lookup"><span data-stu-id="fbfbe-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="fbfbe-123">Il metodo non può contenere **ref** o **out** parametri.</span><span class="sxs-lookup"><span data-stu-id="fbfbe-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="fbfbe-124">Si noti che non sono previste restrizioni sul tipo restituito di un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="fbfbe-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="fbfbe-125">Un'azione del controller può restituire una stringa, un valore DateTime, un'istanza della classe Random, o void.</span><span class="sxs-lookup"><span data-stu-id="fbfbe-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="fbfbe-126">Il framework di MVC ASP.NET verrà convertire un tipo restituito non è il risultato di un'azione in una stringa ed eseguire il rendering di stringa per il browser.</span><span class="sxs-lookup"><span data-stu-id="fbfbe-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="fbfbe-127">Quando si aggiunge un metodo che non violi questi requisiti per un controller, il metodo viene esposta come un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="fbfbe-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="fbfbe-128">Prestare attenzione qui.</span><span class="sxs-lookup"><span data-stu-id="fbfbe-128">Be careful here.</span></span> <span data-ttu-id="fbfbe-129">Un'azione del controller può essere richiamata da qualsiasi utente connesso a Internet.</span><span class="sxs-lookup"><span data-stu-id="fbfbe-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="fbfbe-130">Non, ad esempio, creare un'azione del controller DeleteMyWebsite().</span><span class="sxs-lookup"><span data-stu-id="fbfbe-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="fbfbe-131">Impedisce che viene richiamato un metodo pubblico</span><span class="sxs-lookup"><span data-stu-id="fbfbe-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="fbfbe-132">Se è necessario creare un metodo pubblico in una classe controller e non si desidera esporre il metodo come un'azione del controller, quindi è possibile impedire che il metodo richiamato utilizzando il &lt;NonAction&gt; attributo.</span><span class="sxs-lookup"><span data-stu-id="fbfbe-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the &lt;NonAction&gt; attribute.</span></span> <span data-ttu-id="fbfbe-133">Ad esempio, il controller nel listato 2 contiene un metodo pubblico denominato CompanySecrets() decorata con il &lt;NonAction&gt; attributo.</span><span class="sxs-lookup"><span data-stu-id="fbfbe-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the &lt;NonAction&gt; attribute.</span></span>

<span data-ttu-id="fbfbe-134">**Elenco di 2 - Controllers\WorkController.vb**</span><span class="sxs-lookup"><span data-stu-id="fbfbe-134">**Listing 2 - Controllers\WorkController.vb**</span></span>

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

<span data-ttu-id="fbfbe-135">Se si tenta di richiamare l'azione del controller CompanySecrets() digitando /Work/CompanySecrets nella barra degli indirizzi del browser si otterrà il messaggio di errore nella figura 1.</span><span class="sxs-lookup"><span data-stu-id="fbfbe-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>


<span data-ttu-id="fbfbe-136">[![Richiama un metodo NonAction](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fbfbe-136">[![Invoking a NonAction method](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)</span></span>

<span data-ttu-id="fbfbe-137">**Figura 01**: chiamata di un metodo NonAction ([fare clic per visualizzare l'immagine ingrandita](creating-an-action-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="fbfbe-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-vb/_static/image2.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="fbfbe-138">[Precedente](creating-a-controller-vb.md)
[Successivo](aspnet-mvc-controllers-overview-cs.md)</span><span class="sxs-lookup"><span data-stu-id="fbfbe-138">[Previous](creating-a-controller-vb.md)
[Next](aspnet-mvc-controllers-overview-cs.md)</span></span>