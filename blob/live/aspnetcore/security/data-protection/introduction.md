---
title: Introduzione alla protezione dei dati
author: rick-anderson
description: Questo documento introduce il concetto di protezione dei dati e descrive i principi di progettazione delle API principali ASP.NET associato.
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/introduction
ms.openlocfilehash: b98027ee0e7c63bac23054d7623f28294388dede
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-data-protection"></a><span data-ttu-id="326f2-103">Introduzione alla protezione dei dati</span><span class="sxs-lookup"><span data-stu-id="326f2-103">Introduction to Data Protection</span></span>

<span data-ttu-id="326f2-104">Le applicazioni Web, spesso è necessario archiviare i dati sensibili alla sicurezza.</span><span class="sxs-lookup"><span data-stu-id="326f2-104">Web applications often need to store security-sensitive data.</span></span> <span data-ttu-id="326f2-105">Windows fornisce DPAPI per le applicazioni desktop, ma questo non è adatto per le applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="326f2-105">Windows provides DPAPI for desktop applications but this is unsuitable for web applications.</span></span> <span data-ttu-id="326f2-106">Lo stack di protezione dati di ASP.NET Core forniscono un'API di crittografia semplice e facile da usare uno sviluppatore può utilizzare per proteggere i dati, tra cui la rotazione e la gestione delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="326f2-106">The ASP.NET Core data protection stack provide a simple, easy to use cryptographic API a developer can use to protect data, including key management and rotation.</span></span>

<span data-ttu-id="326f2-107">Lo stack di protezione dati di ASP.NET Core è progettato per essere utilizzato come valore di sostituzione a lungo termine per il <machineKey> elemento ASP.NET 1. x - 4. x.</span><span class="sxs-lookup"><span data-stu-id="326f2-107">The ASP.NET Core data protection stack is designed to serve as the long-term replacement for the <machineKey> element in ASP.NET 1.x - 4.x.</span></span> <span data-ttu-id="326f2-108">È stato progettato per affrontare molti degli svantaggi dello stack di crittografia precedente, offrendo una soluzione di casella per la maggior parte dei casi di utilizzo di applicazioni moderne sono potrebbero verificarsi.</span><span class="sxs-lookup"><span data-stu-id="326f2-108">It was designed to address many of the shortcomings of the old cryptographic stack while providing an out-of-the-box solution for the majority of use cases modern applications are likely to encounter.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="326f2-109">Presentazione del problema</span><span class="sxs-lookup"><span data-stu-id="326f2-109">Problem statement</span></span>

<span data-ttu-id="326f2-110">L'istruzione di problema generale può essere dichiarata in una singola frase sinteticamente: è necessario rendere persistenti le informazioni attendibili per il successivo recupero, ma il meccanismo di persistenza non attendibili.</span><span class="sxs-lookup"><span data-stu-id="326f2-110">The overall problem statement can be succinctly stated in a single sentence: I need to persist trusted information for later retrieval, but I do not trust the persistence mechanism.</span></span> <span data-ttu-id="326f2-111">In termini di web, questo potrebbe essere scritto come "Ho bisogno di round trip stato attendibile tramite un client non attendibile".</span><span class="sxs-lookup"><span data-stu-id="326f2-111">In web terms, this might be written as "I need to round-trip trusted state via an untrusted client."</span></span>

<span data-ttu-id="326f2-112">L'esempio canonico è un cookie di autenticazione o di trasporto token.</span><span class="sxs-lookup"><span data-stu-id="326f2-112">The canonical example of this is an authentication cookie or bearer token.</span></span> <span data-ttu-id="326f2-113">Il server genera un "Sono Groot e disporre delle autorizzazioni xyz" del token e lo passa al client.</span><span class="sxs-lookup"><span data-stu-id="326f2-113">The server generates an "I am Groot and have xyz permissions" token and hands it to the client.</span></span> <span data-ttu-id="326f2-114">In seguito il client presenta il token al server, ma il server necessita di un tipo di garanzia che il client non è ancora contraffatte il token.</span><span class="sxs-lookup"><span data-stu-id="326f2-114">At some future date the client will present that token back to the server, but the server needs some kind of assurance that the client hasn't forged the token.</span></span> <span data-ttu-id="326f2-115">In questo modo il primo requisito: autenticità (anche noto come</span><span class="sxs-lookup"><span data-stu-id="326f2-115">Thus the first requirement: authenticity (a.k.a.</span></span> <span data-ttu-id="326f2-116">integrità, correzione manomissioni).</span><span class="sxs-lookup"><span data-stu-id="326f2-116">integrity, tamper-proofing).</span></span>

<span data-ttu-id="326f2-117">Poiché lo stato persistente è considerato attendibile dal server, si prevede che questo stato può contenere informazioni specifiche per l'ambiente operativo.</span><span class="sxs-lookup"><span data-stu-id="326f2-117">Since the persisted state is trusted by the server, we anticipate that this state might contain information that is specific to the operating environment.</span></span> <span data-ttu-id="326f2-118">È possibile sotto forma di un percorso di file, un'autorizzazione, un handle o altri riferimento indiretto o di altre parti di dati specifici del server.</span><span class="sxs-lookup"><span data-stu-id="326f2-118">This could be in the form of a file path, a permission, a handle or other indirect reference, or some other piece of server-specific data.</span></span> <span data-ttu-id="326f2-119">Tali informazioni in genere non identificabile da un client non attendibile.</span><span class="sxs-lookup"><span data-stu-id="326f2-119">Such information should generally not be disclosed to an untrusted client.</span></span> <span data-ttu-id="326f2-120">In questo modo il secondo requisito: riservatezza.</span><span class="sxs-lookup"><span data-stu-id="326f2-120">Thus the second requirement: confidentiality.</span></span>

<span data-ttu-id="326f2-121">Infine, poiché sono componenti applicazioni moderne, abbiamo visto è che singoli componenti verranno possa sfruttare i vantaggi di questo sistema senza tenere in considerazione altri componenti nel sistema.</span><span class="sxs-lookup"><span data-stu-id="326f2-121">Finally, since modern applications are componentized, what we've seen is that individual components will want to take advantage of this system without regard to other components in the system.</span></span> <span data-ttu-id="326f2-122">Ad esempio, se un componente di token di connessione, viene utilizzato questo stack, devono operare senza interferenze da un meccanismo di anti-CSRF che potrebbe anche usare lo stesso stack.</span><span class="sxs-lookup"><span data-stu-id="326f2-122">For instance, if a bearer token component is using this stack, it should operate without interference from an anti-CSRF mechanism that might also be using the same stack.</span></span> <span data-ttu-id="326f2-123">In questo modo il requisito finale: isolamento.</span><span class="sxs-lookup"><span data-stu-id="326f2-123">Thus the final requirement: isolation.</span></span>

<span data-ttu-id="326f2-124">È possibile fornire ulteriori vincoli per limitare l'ambito dei requisiti.</span><span class="sxs-lookup"><span data-stu-id="326f2-124">We can provide further constraints in order to narrow the scope of our requirements.</span></span> <span data-ttu-id="326f2-125">Si presuppone che tutti i servizi operando all'interno di sistema di crittografia sono ugualmente attendibili e che i dati non debbano essere generato o utilizzato all'esterno di servizi sotto il controllo diretto.</span><span class="sxs-lookup"><span data-stu-id="326f2-125">We assume that all services operating within the cryptosystem are equally trusted and that the data does not need to be generated or consumed outside of the services under our direct control.</span></span> <span data-ttu-id="326f2-126">Inoltre, è necessario che le operazioni sono più rapidamente possibile poiché ogni richiesta al servizio web può passare attraverso il sistema di crittografia una o più volte.</span><span class="sxs-lookup"><span data-stu-id="326f2-126">Furthermore, we require that operations are as fast as possible since each request to the web service might go through the cryptosystem one or more times.</span></span> <span data-ttu-id="326f2-127">In questo modo la soluzione ideale per questo scenario la crittografia simmetrica ed è possibile sconto crittografia asimmetrica fino ad una volta che è necessario.</span><span class="sxs-lookup"><span data-stu-id="326f2-127">This makes symmetric cryptography ideal for our scenario, and we can discount asymmetric cryptography until such a time that it is needed.</span></span>

## <a name="design-philosophy"></a><span data-ttu-id="326f2-128">Filosofia di progettazione</span><span class="sxs-lookup"><span data-stu-id="326f2-128">Design philosophy</span></span>

<span data-ttu-id="326f2-129">Abbiamo iniziato identificando i problemi con lo stack di esistente.</span><span class="sxs-lookup"><span data-stu-id="326f2-129">We started by identifying problems with the existing stack.</span></span> <span data-ttu-id="326f2-130">Una volta che abbiamo, è un sondaggio orizzontale delle soluzioni esistenti e ha rilevato che nessuna soluzione era piuttosto la funzionalità che è richiesto.</span><span class="sxs-lookup"><span data-stu-id="326f2-130">Once we had that, we surveyed the landscape of existing solutions and concluded that no existing solution quite had the capabilities we sought.</span></span> <span data-ttu-id="326f2-131">È quindi progettato una soluzione basata su orientamenti diversi.</span><span class="sxs-lookup"><span data-stu-id="326f2-131">We then engineered a solution based on several guiding principles.</span></span>

* <span data-ttu-id="326f2-132">Il sistema deve offrire semplicità di configurazione.</span><span class="sxs-lookup"><span data-stu-id="326f2-132">The system should offer simplicity of configuration.</span></span> <span data-ttu-id="326f2-133">Idealmente il sistema sarà senza operazioni di configurazione e gli sviluppatori Impossibile hit interamente in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="326f2-133">Ideally the system would be zero-configuration and developers could hit the ground running.</span></span> <span data-ttu-id="326f2-134">Situazioni in cui gli sviluppatori devono configurare un aspetto specifico (ad esempio l'archivio chiave), debba essere presi in considerazione semplificare tali configurazioni specifiche.</span><span class="sxs-lookup"><span data-stu-id="326f2-134">In situations where developers need to configure a specific aspect (such as the key repository), consideration should be given to making those specific configurations simple.</span></span>

* <span data-ttu-id="326f2-135">Offre una semplice API per consumatori.</span><span class="sxs-lookup"><span data-stu-id="326f2-135">Offer a simple consumer-facing API.</span></span> <span data-ttu-id="326f2-136">Le API devono essere facile da utilizzare in modo corretto e difficile da usare in modo non corretto.</span><span class="sxs-lookup"><span data-stu-id="326f2-136">The APIs should be easy to use correctly and difficult to use incorrectly.</span></span>

* <span data-ttu-id="326f2-137">Gli sviluppatori non imparare principi di gestione delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="326f2-137">Developers should not learn key management principles.</span></span> <span data-ttu-id="326f2-138">Il sistema deve gestire selezione algoritmo e la durata di chiave per conto dello sviluppatore.</span><span class="sxs-lookup"><span data-stu-id="326f2-138">The system should handle algorithm selection and key lifetime on the developer's behalf.</span></span> <span data-ttu-id="326f2-139">Idealmente lo sviluppatore non è nemmeno dovrebbe disporre di accesso al materiale della chiave non elaborato.</span><span class="sxs-lookup"><span data-stu-id="326f2-139">Ideally the developer should never even have access to the raw key material.</span></span>

* <span data-ttu-id="326f2-140">Le chiavi devono essere protetti inattivi quando possibile.</span><span class="sxs-lookup"><span data-stu-id="326f2-140">Keys should be protected at rest when possible.</span></span> <span data-ttu-id="326f2-141">Il sistema deve individuare un meccanismo di protezione predefinito appropriato e applicarlo automaticamente.</span><span class="sxs-lookup"><span data-stu-id="326f2-141">The system should figure out an appropriate default protection mechanism and apply it automatically.</span></span>

<span data-ttu-id="326f2-142">Con questi principi presente è stato sviluppato un semplice, [facile da usare](using-data-protection.md) dello stack di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="326f2-142">With these principles in mind we developed a simple, [easy to use](using-data-protection.md) data protection stack.</span></span>

<span data-ttu-id="326f2-143">Le API di protezione dati ASP.NET Core non sono principalmente destinati indefinita persistenza del payload riservato.</span><span class="sxs-lookup"><span data-stu-id="326f2-143">The ASP.NET Core data protection APIs are not primarily intended for indefinite persistence of confidential payloads.</span></span> <span data-ttu-id="326f2-144">Altre tecnologie come [DPAPI CNG di Windows](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) e [Azure Rights Management](https://docs.microsoft.com/rights-management/) sono più adatti per lo scenario di memorizzazione indefinito, e hanno funzionalità di gestione delle chiavi conseguentemente sicuro.</span><span class="sxs-lookup"><span data-stu-id="326f2-144">Other technologies like [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) and [Azure Rights Management](https://docs.microsoft.com/rights-management/) are more suited to the scenario of indefinite storage, and they have correspondingly strong key management capabilities.</span></span> <span data-ttu-id="326f2-145">Ciò premesso, non c'è niente che uno sviluppatore di utilizzare le API di protezione dati ASP.NET Core per la protezione a lungo termine dei dati riservati.</span><span class="sxs-lookup"><span data-stu-id="326f2-145">That said, there is nothing prohibiting a developer from using the ASP.NET Core data protection APIs for long-term protection of confidential data.</span></span>

## <a name="audience"></a><span data-ttu-id="326f2-146">Destinatari</span><span class="sxs-lookup"><span data-stu-id="326f2-146">Audience</span></span>

<span data-ttu-id="326f2-147">Il sistema di protezione dati è suddivisa in cinque pacchetti principali.</span><span class="sxs-lookup"><span data-stu-id="326f2-147">The data protection system is divided into five main packages.</span></span> <span data-ttu-id="326f2-148">Vari aspetti di queste API destinazione tre gruppi di destinatari principale;</span><span class="sxs-lookup"><span data-stu-id="326f2-148">Various aspects of these APIs target three main audiences;</span></span>

1. <span data-ttu-id="326f2-149">Il [Consumer API Panoramica](consumer-apis/overview.md) gli sviluppatori di applicazioni e framework di destinazione.</span><span class="sxs-lookup"><span data-stu-id="326f2-149">The [Consumer APIs Overview](consumer-apis/overview.md) target application and framework developers.</span></span>

   <span data-ttu-id="326f2-150">"Non si desidera ulteriori informazioni sulla modalità di funzionamento dello stack o sul modo in cui è configurato.</span><span class="sxs-lookup"><span data-stu-id="326f2-150">"I don't want to learn about how the stack operates or about how it is configured.</span></span> <span data-ttu-id="326f2-151">Desidera semplicemente eseguire alcune operazioni in come semplice modo possibile con elevata probabilità di utilizzando le API correttamente."</span><span class="sxs-lookup"><span data-stu-id="326f2-151">I simply want to perform some operation in as simple a manner as possible with high probability of using the APIs successfully."</span></span>

2. <span data-ttu-id="326f2-152">Il [le API di configurazione](configuration/overview.md) gli sviluppatori di applicazioni e gli amministratori di sistema di destinazione.</span><span class="sxs-lookup"><span data-stu-id="326f2-152">The [configuration APIs](configuration/overview.md) target application developers and system administrators.</span></span>

   <span data-ttu-id="326f2-153">"È necessario che l'ambiente richiede i percorsi non predefiniti o le impostazioni per il sistema di protezione dati".</span><span class="sxs-lookup"><span data-stu-id="326f2-153">"I need to tell the data protection system that my environment requires non-default paths or settings."</span></span>

3. <span data-ttu-id="326f2-154">Gli sviluppatori di destinazione API extensibility responsabile implementazione dei criteri personalizzati.</span><span class="sxs-lookup"><span data-stu-id="326f2-154">The extensibility APIs target developers in charge of implementing custom policy.</span></span> <span data-ttu-id="326f2-155">Utilizzo di queste API limitato a determinate situazioni e si è verificato, gli sviluppatori compatibile con protezione.</span><span class="sxs-lookup"><span data-stu-id="326f2-155">Usage of these APIs would be limited to rare situations and experienced, security aware developers.</span></span>

   <span data-ttu-id="326f2-156">"È necessario sostituire un intero componente all'interno del sistema perché ho requisiti comportamentali realmente univoci.</span><span class="sxs-lookup"><span data-stu-id="326f2-156">"I need to replace an entire component within the system because I have truly unique behavioral requirements.</span></span> <span data-ttu-id="326f2-157">Desidero informazioni uncommonly parti della superficie dell'API per compilare un plug-in che soddisfa i requisiti."</span><span class="sxs-lookup"><span data-stu-id="326f2-157">I am willing to learn uncommonly-used parts of the API surface in order to build a plugin that fulfills my requirements."</span></span>

## <a name="package-layout"></a><span data-ttu-id="326f2-158">Layout del pacchetto</span><span class="sxs-lookup"><span data-stu-id="326f2-158">Package Layout</span></span>

<span data-ttu-id="326f2-159">Lo stack di protezione dati è costituito da cinque pacchetti.</span><span class="sxs-lookup"><span data-stu-id="326f2-159">The data protection stack consists of five packages.</span></span>

* <span data-ttu-id="326f2-160">Microsoft.AspNetCore.DataProtection.Abstractions contiene le interfacce di base IDataProtectionProvider e oggetto IDataProtector.</span><span class="sxs-lookup"><span data-stu-id="326f2-160">Microsoft.AspNetCore.DataProtection.Abstractions contains the basic IDataProtectionProvider and IDataProtector interfaces.</span></span> <span data-ttu-id="326f2-161">Contiene inoltre i metodi di estensione utili che consentono l'utilizzo di questi tipi (ad esempio, gli overload di IDataProtector.Protect).</span><span class="sxs-lookup"><span data-stu-id="326f2-161">It also contains useful extension methods that can assist working with these types (e.g., overloads of IDataProtector.Protect).</span></span> <span data-ttu-id="326f2-162">Vedere la sezione di interfacce utente per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="326f2-162">See the consumer interfaces section for more information.</span></span> <span data-ttu-id="326f2-163">Se un altro utente è responsabile della creazione di un'istanza nel sistema di protezione dati e si utilizza semplicemente le API, è opportuno Microsoft.AspNetCore.DataProtection.Abstractions di riferimento.</span><span class="sxs-lookup"><span data-stu-id="326f2-163">If somebody else is responsible for instantiating the data protection system and you are simply consuming the APIs, you'll want to reference Microsoft.AspNetCore.DataProtection.Abstractions.</span></span>

* <span data-ttu-id="326f2-164">Microsoft.AspNetCore.DataProtection contiene l'implementazione di base del sistema di protezione dati, compresi configurazione, estendibilità, gestione delle chiavi e operazioni di crittografia di base.</span><span class="sxs-lookup"><span data-stu-id="326f2-164">Microsoft.AspNetCore.DataProtection contains the core implementation of the data protection system, including the core cryptographic operations, key management, configuration, and extensibility.</span></span> <span data-ttu-id="326f2-165">Se si è responsabile della creazione di un'istanza nel sistema di protezione dati (ad esempio, aggiungerlo a un IServiceCollection) o modificare o estendere il comportamento, è opportuno Microsoft.AspNetCore.DataProtection di riferimento.</span><span class="sxs-lookup"><span data-stu-id="326f2-165">If you're responsible for instantiating the data protection system (e.g., adding it to an IServiceCollection) or modifying or extending its behavior, you'll want to reference Microsoft.AspNetCore.DataProtection.</span></span>

* <span data-ttu-id="326f2-166">Microsoft.AspNetCore.DataProtection.Extensions contiene API aggiuntive, quali gli sviluppatori possono risultare utili ma che non appartengono il pacchetto principale.</span><span class="sxs-lookup"><span data-stu-id="326f2-166">Microsoft.AspNetCore.DataProtection.Extensions contains additional APIs which developers might find useful but which don't belong in the core package.</span></span> <span data-ttu-id="326f2-167">Ad esempio, il pacchetto contiene un'API di semplice "creare un'istanza del sistema punta a una directory di archiviazione di chiavi specifico con alcuna installazione di aggiunta della dipendenza" (informazioni).</span><span class="sxs-lookup"><span data-stu-id="326f2-167">For instance, this package contains a simple "instantiate the system pointing at a specific key storage directory with no dependency injection setup" API (more info).</span></span> <span data-ttu-id="326f2-168">Contiene inoltre i metodi di estensione per limitare la durata del payload protetto (informazioni).</span><span class="sxs-lookup"><span data-stu-id="326f2-168">It also contains extension methods for limiting the lifetime of protected payloads (more info).</span></span>

* <span data-ttu-id="326f2-169">Microsoft.AspNetCore.DataProtection.SystemWeb può essere installato in un'applicazione di 4. x ASP.NET esistente per reindirizzare il <machineKey> operazioni per usare il nuovo stack di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="326f2-169">Microsoft.AspNetCore.DataProtection.SystemWeb can be installed into an existing ASP.NET 4.x application to redirect its <machineKey> operations to instead use the new data protection stack.</span></span> <span data-ttu-id="326f2-170">Vedere [compatibilità](compatibility/replacing-machinekey.md#compatibility-replacing-machinekey) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="326f2-170">See [compatibility](compatibility/replacing-machinekey.md#compatibility-replacing-machinekey) for more information.</span></span>

* <span data-ttu-id="326f2-171">Microsoft.AspNetCore.Cryptography.KeyDerivation fornisce un'implementazione della password PBKDF2 hashing routine e può essere utilizzato dai sistemi che necessitano di gestire in modo sicuro le password utente.</span><span class="sxs-lookup"><span data-stu-id="326f2-171">Microsoft.AspNetCore.Cryptography.KeyDerivation provides an implementation of the PBKDF2 password hashing routine and can be used by systems which need to handle user passwords securely.</span></span> <span data-ttu-id="326f2-172">Vedere [Hashing della Password](consumer-apis/password-hashing.md) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="326f2-172">See [Password Hashing](consumer-apis/password-hashing.md) for more information.</span></span>