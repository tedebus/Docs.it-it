---
title: DevOps con ASP.NET Core e Azure | Strumenti e download
author: CamSoper
description: Questa guida include informazioni complete sulla creazione di una pipeline DevOps per un'app ASP.NET Core ospitata in Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: a63e97d9ab9eb0ed2fbd30e8c2e033f0c048d33e
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312301"
---
# <a name="tools-and-downloads"></a><span data-ttu-id="f2383-103">Strumenti e download</span><span class="sxs-lookup"><span data-stu-id="f2383-103">Tools and downloads</span></span>

<span data-ttu-id="f2383-104">Azure offre diverse interfacce per il provisioning e gestione delle risorse, ad esempio la [portale di Azure](https://portal.azure.com), [CLI di Azure](https://docs.microsoft.com/cli/azure/), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), [Cloud di Azure Shell](https://shell.azure.com/bash)e Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f2383-104">Azure has several interfaces for provisioning and managing resources, such as the [Azure portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/cli/azure/), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash), and Visual Studio.</span></span> <span data-ttu-id="f2383-105">Questa Guida adotta un approccio minimalista e Usa Azure Cloud Shell ogni volta che è possibile ridurre i passaggi necessari.</span><span class="sxs-lookup"><span data-stu-id="f2383-105">This guide takes a minimalist approach and uses the Azure Cloud Shell whenever possible to reduce the steps required.</span></span> <span data-ttu-id="f2383-106">Tuttavia, il portale di Azure deve essere usato per alcune parti.</span><span class="sxs-lookup"><span data-stu-id="f2383-106">However, the Azure portal must be used for some portions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f2383-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="f2383-107">Prerequisites</span></span>

<span data-ttu-id="f2383-108">Le sottoscrizioni seguenti sono necessari:</span><span class="sxs-lookup"><span data-stu-id="f2383-108">The following subscriptions are required:</span></span>

* <span data-ttu-id="f2383-109">Azure &mdash; se non hai un account [ottenere una versione di valutazione gratuita](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="f2383-109">Azure &mdash; If you don't have an account, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="f2383-110">Visual Studio Team Services (VSTS) &mdash; questo account viene creato nel capitolo 4.</span><span class="sxs-lookup"><span data-stu-id="f2383-110">Visual Studio Team Services (VSTS) &mdash; This account is created in Chapter 4.</span></span>
* <span data-ttu-id="f2383-111">GitHub &mdash; se non hai un account [Iscriviti gratuitamente](https://github.com/join).</span><span class="sxs-lookup"><span data-stu-id="f2383-111">GitHub &mdash; If you don't have an account, [sign up for free](https://github.com/join).</span></span>

<span data-ttu-id="f2383-112">Gli strumenti seguenti sono necessari:</span><span class="sxs-lookup"><span data-stu-id="f2383-112">The following tools are required:</span></span>

* <span data-ttu-id="f2383-113">[GIT](https://git-scm.com/downloads) &mdash; acquisire familiarità con Git è consigliato per questa Guida.</span><span class="sxs-lookup"><span data-stu-id="f2383-113">[Git](https://git-scm.com/downloads) &mdash; A fundamental understanding of Git is recommended for this guide.</span></span> <span data-ttu-id="f2383-114">Rivedere le [documentazione su Git](https://git-scm.com/doc), in particolare [git remoto](https://git-scm.com/docs/git-remote) e [git push](https://git-scm.com/docs/git-push).</span><span class="sxs-lookup"><span data-stu-id="f2383-114">Review the [Git documentation](https://git-scm.com/doc), specifically [git remote](https://git-scm.com/docs/git-remote) and [git push](https://git-scm.com/docs/git-push).</span></span>
* <span data-ttu-id="f2383-115">[.NET core SDK](https://www.microsoft.com/net/download/) &mdash; versione 2.1.300 o in un secondo momento, è necessario per compilare ed eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="f2383-115">[.NET Core SDK](https://www.microsoft.com/net/download/) &mdash; Version 2.1.300 or later is required to build and run the sample app.</span></span> <span data-ttu-id="f2383-116">Se è installato Visual Studio con il **sviluppo multipiattaforma .NET Core** carico di lavoro, .NET Core SDK è già installato.</span><span class="sxs-lookup"><span data-stu-id="f2383-116">If Visual Studio is installed with the **.NET Core cross-platform development** workload, the .NET Core SDK is already installed.</span></span>

    <span data-ttu-id="f2383-117">Verificare l'installazione di .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="f2383-117">Verify your .NET Core SDK installation.</span></span> <span data-ttu-id="f2383-118">Aprire una shell dei comandi ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="f2383-118">Open a command shell, and run the following command:</span></span>

    ```console
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a><span data-ttu-id="f2383-119">Strumenti consigliati (solo Windows)</span><span class="sxs-lookup"><span data-stu-id="f2383-119">Recommended tools (Windows only)</span></span>

* <span data-ttu-id="f2383-120">[Visual Studio](https://www.visualstudio.com/)di strumenti di Azure affidabili forniscono un'interfaccia utente grafica per la maggior parte delle funzionalità descritte in questa Guida.</span><span class="sxs-lookup"><span data-stu-id="f2383-120">[Visual Studio](https://www.visualstudio.com/)'s robust Azure tools provide a GUI for most of the functionality described in this guide.</span></span> <span data-ttu-id="f2383-121">Qualsiasi edizione di Visual Studio funzionerà, tra cui l'edizione gratuita di Visual Studio Community.</span><span class="sxs-lookup"><span data-stu-id="f2383-121">Any edition of Visual Studio will work, including the free Visual Studio Community Edition.</span></span> <span data-ttu-id="f2383-122">Le esercitazioni vengono scritti per illustrare lo sviluppo, distribuzione e DevOps con e senza Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f2383-122">The tutorials are written to demonstrate development, deployment, and DevOps both with and without Visual Studio.</span></span>

  <span data-ttu-id="f2383-123">Verificare che Visual Studio offre i seguenti [carichi di lavoro](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) installato:</span><span class="sxs-lookup"><span data-stu-id="f2383-123">Confirm that Visual Studio has the following [workloads](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) installed:</span></span>

  * <span data-ttu-id="f2383-124">Sviluppo ASP.NET e Web</span><span class="sxs-lookup"><span data-stu-id="f2383-124">ASP.NET and web development</span></span>
  * <span data-ttu-id="f2383-125">Sviluppo di Azure</span><span class="sxs-lookup"><span data-stu-id="f2383-125">Azure development</span></span>
  * <span data-ttu-id="f2383-126">Sviluppo multipiattaforma .NET Core</span><span class="sxs-lookup"><span data-stu-id="f2383-126">.NET Core cross-platform development</span></span>