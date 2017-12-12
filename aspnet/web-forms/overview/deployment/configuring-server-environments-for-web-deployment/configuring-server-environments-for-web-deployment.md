---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: Configurazione degli ambienti di Server per la distribuzione Web | Documenti Microsoft
author: jrjlee
description: In questa esercitazione viene illustrato come configurare gli ambienti server a supporto di un solo clic o automatizzato, distribuzione del sito Web e la pubblicazione in vari dello scenario diverse...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 8a07d283e3e4344e5513152cf760ac90481d9f4b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="configuring-server-environments-for-web-deployment"></a><span data-ttu-id="633c7-103">Configurazione degli ambienti di Server per la distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="633c7-103">Configuring Server Environments for Web Deployment</span></span>
====================
<span data-ttu-id="633c7-104">da [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="633c7-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="633c7-105">Scarica il PDF</span><span class="sxs-lookup"><span data-stu-id="633c7-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="633c7-106">In questa esercitazione Mostra come configurare gli ambienti server a un solo clic, o automatizzati, distribuzione del sito Web e la pubblicazione in vari scenari diversi.</span><span class="sxs-lookup"><span data-stu-id="633c7-106">This tutorial will show you how to set up server environments to support one-click, or automated, website deployment and publishing in various different scenarios.</span></span> <span data-ttu-id="633c7-107">L'esercitazione include gli argomenti che consentono di completare varie attività, ad esempio la configurazione di un server web per approcci specifici per la distribuzione e configurazione di una server farm Web Farm Framework (WFF), insieme a panoramiche basata su scenario che forniscono supporto istruzioni di livello superiore end-to-end.</span><span class="sxs-lookup"><span data-stu-id="633c7-107">The tutorial includes topics to walk you through completing various tasks, like configuring a web server to support specific approaches to deployment and setting up a Web Farm Framework (WFF) server farm, together with scenario-based overviews that provide higher-level end-to-end guidance.</span></span>
> 
> <span data-ttu-id="633c7-108">L'esercitazione Usa lo scenario di distribuzione di Fabrikam, Inc. descritto in [distribuzione Web aziendale: panoramica dello Scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) come punto di riferimento per gli esempi e l'infrastruttura di rete.</span><span class="sxs-lookup"><span data-stu-id="633c7-108">The tutorial uses the Fabrikam, Inc. deployment scenario described in [Enterprise Web Deployment: Scenario Overview](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) as a reference point for examples and network infrastructure.</span></span>
> 
> <span data-ttu-id="633c7-109">Per una traduzione italiana di queste esercitazioni, visitare [http://www.lucamorelli.it](http://www.lucamorelli.it).</span><span class="sxs-lookup"><span data-stu-id="633c7-109">For an Italian translation of these tutorials, visit [http://www.lucamorelli.it](http://www.lucamorelli.it).</span></span>


<span data-ttu-id="633c7-110">In questa esercitazione include questi argomenti:</span><span class="sxs-lookup"><span data-stu-id="633c7-110">This tutorial includes these topics:</span></span>

- [<span data-ttu-id="633c7-111">Scelta dell'approccio corretto per la distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="633c7-111">Choosing the Right Approach to Web Deployment</span></span>](choosing-the-right-approach-to-web-deployment.md)
- [<span data-ttu-id="633c7-112">Scenario: Configurazione di un ambiente di Test per la distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="633c7-112">Scenario: Configuring a Test Environment for Web Deployment</span></span>](scenario-configuring-a-test-environment-for-web-deployment.md)
- [<span data-ttu-id="633c7-113">Scenario: Configurazione di un ambiente di gestione temporanea per la distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="633c7-113">Scenario: Configuring a Staging Environment for Web Deployment</span></span>](scenario-configuring-a-staging-environment-for-web-deployment.md)
- [<span data-ttu-id="633c7-114">Scenario: Configurazione di un ambiente di produzione per la distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="633c7-114">Scenario: Configuring a Production Environment for Web Deployment</span></span>](scenario-configuring-a-production-environment-for-web-deployment.md)
- [<span data-ttu-id="633c7-115">Configurazione di un Server Web per la pubblicazione (agente remoto) di distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="633c7-115">Configuring a Web Server for Web Deploy Publishing (Remote Agent)</span></span>](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
- [<span data-ttu-id="633c7-116">Configurazione di un Server Web per il Web la pubblicazione di distribuzione (gestore distribuzione Web)</span><span class="sxs-lookup"><span data-stu-id="633c7-116">Configuring a Web Server for Web Deploy Publishing (Web Deploy Handler)</span></span>](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
- [<span data-ttu-id="633c7-117">Configurazione di un Server Web per la pubblicazione (distribuzione Offline) di distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="633c7-117">Configuring a Web Server for Web Deploy Publishing (Offline Deployment)</span></span>](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
- [<span data-ttu-id="633c7-118">Configurazione di un Server di Database per la pubblicazione di distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="633c7-118">Configuring a Database Server for Web Deploy Publishing</span></span>](configuring-a-database-server-for-web-deploy-publishing.md)
- [<span data-ttu-id="633c7-119">Creazione di una Server Farm con Web Farm Framework</span><span class="sxs-lookup"><span data-stu-id="633c7-119">Creating a Server Farm with the Web Farm Framework</span></span>](creating-a-server-farm-with-the-web-farm-framework.md)
- [<span data-ttu-id="633c7-120">Configurazione delle proprietà di distribuzione di un ambiente di destinazione</span><span class="sxs-lookup"><span data-stu-id="633c7-120">Configuring Deployment Properties for a Target Environment</span></span>](configuring-deployment-properties-for-a-target-environment.md)

<span data-ttu-id="633c7-121">Il primo argomento, [scelta dell'approccio di destra per distribuzione Web](choosing-the-right-approach-to-web-deployment.md), vengono descritti gli approcci principali, è possibile utilizzare per pubblicare applicazioni web tramite lo strumento di distribuzione Web di Internet Information Services (IIS) (distribuzione Web) 2.0.</span><span class="sxs-lookup"><span data-stu-id="633c7-121">The first topic, [Choosing the Right Approach to Web Deployment](choosing-the-right-approach-to-web-deployment.md), describes the main approaches you can use to publish web applications by using the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) 2.0.</span></span> <span data-ttu-id="633c7-122">Vengono inoltre identificati gli scenari in cui eseguire il mapping a ciascun approccio.</span><span class="sxs-lookup"><span data-stu-id="633c7-122">It also identifies the scenarios that map to each approach.</span></span> <span data-ttu-id="633c7-123">Da qui, ogni argomento di scenario viene fornita una panoramica delle attività da completare e identifica gli argomenti in cui che è necessario per completare queste attività di lavoro tramite.</span><span class="sxs-lookup"><span data-stu-id="633c7-123">From here, each scenario topic provides a high-level overview of the tasks you need to complete and identifies the topics you'll need to work through to help you complete these tasks.</span></span>

<span data-ttu-id="633c7-124">Se si utilizza l'approccio di file di progetto split descritto in [comprendere il processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md) per compilare e distribuire la soluzione, l'argomento finale, [configurazione delle proprietà di distribuzione per un ambiente di destinazione](configuring-deployment-properties-for-a-target-environment.md), viene descritto come configurare i file di progetto specifici dell'ambiente per la distribuzione in ambienti di destinazione diversi.</span><span class="sxs-lookup"><span data-stu-id="633c7-124">If you're using the split project file approach described in [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md) to build and deploy your solution, the final topic, [Configuring Deployment Properties for a Target Environment](configuring-deployment-properties-for-a-target-environment.md), describes how to configure environment-specific project files for deployment to different destination environments.</span></span>

## <a name="key-technologies"></a><span data-ttu-id="633c7-125">Tecnologie principali</span><span class="sxs-lookup"><span data-stu-id="633c7-125">Key Technologies</span></span>

<span data-ttu-id="633c7-126">In questa esercitazione viene illustrato come utilizzare questi prodotti e tecnologie per supportare la distribuzione web:</span><span class="sxs-lookup"><span data-stu-id="633c7-126">This tutorial focuses on how to use these products and technologies to support web deployment:</span></span>

- <span data-ttu-id="633c7-127">IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="633c7-127">IIS 7.5</span></span>
- <span data-ttu-id="633c7-128">2. x di distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="633c7-128">Web Deploy 2.x</span></span>
- <span data-ttu-id="633c7-129">WFF 2. x</span><span class="sxs-lookup"><span data-stu-id="633c7-129">WFF 2.x</span></span>
- <span data-ttu-id="633c7-130">Servizio di gestione IIS Web (WMSvc)</span><span class="sxs-lookup"><span data-stu-id="633c7-130">IIS Web Management Service (WMSvc)</span></span>

<span data-ttu-id="633c7-131">L'esercitazione interessa anche l'utilizzo di ASP.NET MVC 3, SQL Server 2008 R2, ASP.NET 4.0 e Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="633c7-131">The tutorial also touches on the use of Windows Server 2008 R2, SQL Server 2008 R2, ASP.NET 4.0, and ASP.NET MVC 3.</span></span>

## <a name="other-tutorials-in-this-series"></a><span data-ttu-id="633c7-132">Altre esercitazioni di questa serie</span><span class="sxs-lookup"><span data-stu-id="633c7-132">Other Tutorials in This Series</span></span>

<span data-ttu-id="633c7-133">Questo fa parte di una serie di cinque esercitazioni su distribuzione web su larga scala.</span><span class="sxs-lookup"><span data-stu-id="633c7-133">This forms part of a series of five tutorials on enterprise-scale web deployment.</span></span> <span data-ttu-id="633c7-134">Ecco le altre esercitazioni nella serie:</span><span class="sxs-lookup"><span data-stu-id="633c7-134">These are the other tutorials in the series:</span></span>

- <span data-ttu-id="633c7-135">[Distribuzione di applicazioni Web in scenari aziendali](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="633c7-135">[Deploying Web Applications in Enterprise Scenarios](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).</span></span> <span data-ttu-id="633c7-136">Questo contenuto introduttivo fornisce lo sfondo di scelta rapida per la serie di esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="633c7-136">This introductory content provides the contextual background for the tutorial series.</span></span> <span data-ttu-id="633c7-137">Viene descritto lo scenario dell'esercitazione e viene illustrato come le attività e procedure dettagliate descritte in tutta la serie rientrano in un processo più ampio di Application Lifecycle Management (ALM).</span><span class="sxs-lookup"><span data-stu-id="633c7-137">It describes the tutorial scenario, and it illustrates how the tasks and walkthroughs described throughout the series fit into a broader Application Lifecycle Management (ALM) process.</span></span>
- <span data-ttu-id="633c7-138">[Distribuzione nell'organizzazione Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).</span><span class="sxs-lookup"><span data-stu-id="633c7-138">[Web Deployment in the Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).</span></span> <span data-ttu-id="633c7-139">In questa esercitazione vengono introdotti i concetti relativi ai file di progetto di Microsoft Build Engine (MSBuild), la Pipeline di pubblicazione sul Web, distribuzione Web e altre tecnologie correlate.</span><span class="sxs-lookup"><span data-stu-id="633c7-139">This tutorial provides a conceptual introduction to Microsoft Build Engine (MSBuild) project files, the Web Publishing Pipeline, Web Deploy, and other related technologies.</span></span> <span data-ttu-id="633c7-140">Spiega come è possibile utilizzare questi strumenti insieme per gestire i processi di distribuzione complessi.</span><span class="sxs-lookup"><span data-stu-id="633c7-140">It explains how you can use these tools together to manage complex deployment processes.</span></span>
- <span data-ttu-id="633c7-141">[Configurazione di Team Foundation Server per la distribuzione Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="633c7-141">[Configuring Team Foundation Server for Web Deployment](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).</span></span> <span data-ttu-id="633c7-142">In questa esercitazione viene descritto come configurare Team Foundation Server (TFS) per supportare diversi scenari di distribuzione, tra cui la distribuzione automatica come parte di un processo di integrazione continua (CI) e attivate manualmente le distribuzioni di compilazioni specifiche.</span><span class="sxs-lookup"><span data-stu-id="633c7-142">This tutorial describes how to configure Team Foundation Server (TFS) to support various deployment scenarios, including automated deployment as part of a continuous integration (CI) process and manually triggered deployments of specific builds.</span></span>
- <span data-ttu-id="633c7-143">[Distribuzione Web aziendale avanzate](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="633c7-143">[Advanced Enterprise Web Deployment](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md).</span></span> <span data-ttu-id="633c7-144">In questa esercitazione viene illustrato come eseguire diverse attività di distribuzione più avanzate, ad esempio personalizzare le distribuzioni di database per più ambienti, esclusione dalla distribuzione di file e cartelle e l'esecuzione di applicazioni web offline durante il processo di distribuzione .</span><span class="sxs-lookup"><span data-stu-id="633c7-144">This tutorial describes how to accomplish various more advanced deployment tasks, like customizing database deployments for multiple environments, excluding files and folders from deployment, and taking web applications offline during the deployment process.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="633c7-145">Successivo</span><span class="sxs-lookup"><span data-stu-id="633c7-145">Next</span></span>](choosing-the-right-approach-to-web-deployment.md)