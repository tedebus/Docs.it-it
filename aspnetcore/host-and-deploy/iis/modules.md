---
title: Uso di moduli IIS con ASP.NET Core
author: guardrex
description: Documento di riferimento che descrive i moduli IIS attivi e inattivi per le applicazioni ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/08/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/modules
ms.openlocfilehash: 1b5391c113ca0b980eb3c47bcce0717d4a4739ed
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="using-iis-modules-with-aspnet-core"></a>Uso di moduli IIS con ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

Le applicazioni ASP.NET Core sono ospitate da IIS in una configurazione di proxy inverso. Alcuni dei moduli nativi IIS e tutti i moduli IIS gestiti non sono disponibili per elaborare le richieste per le applicazioni ASP.NET Core. In molti casi, ASP.NET Core offre un'alternativa alle funzionalità native e gestite dei moduli di IIS.

## <a name="native-modules"></a>Moduli nativi

Modulo | .NET core attivo | Opzione di ASP.NET Core
--- | :---: | ---
**Autenticazione anonima**<br>`AnonymousAuthenticationModule` | Yes | 
**Autenticazione base**<br>`BasicAuthenticationModule` | Yes | 
**Autenticazione Mapping certificazione di client**<br>`CertificateMappingAuthenticationModule` | Yes | 
**CGI**<br>`CgiModule` | No | 
**Convalida della configurazione**<br>`ConfigurationValidationModule` | Yes | 
**Errori HTTP**<br>`CustomErrorModule` | No | [Middleware pagine codice di stato](xref:fundamentals/error-handling#configuring-status-code-pages)
**Registrazioni personalizzate**<br>`CustomLoggingModule` | Yes | 
**Documento predefinito**<br>`DefaultDocumentModule` | No | [Middleware di file predefinito](xref:fundamentals/static-files#serve-a-default-document)
**Autenticazione del digest**<br>`DigestAuthenticationModule` | Yes | 
**Esplorazione directory**<br>`DirectoryListingModule` | No | [Middleware di esplorazione directory](xref:fundamentals/static-files#enable-directory-browsing)
**Compressione dinamica**<br>`DynamicCompressionModule` | Yes | [Middleware di compressione delle risposte](xref:performance/response-compression)
**Traccia**<br>`FailedRequestsTracingModule` | Yes | [Registrazione di ASP.NET Core](xref:fundamentals/logging/index#the-tracesource-provider)
**La memorizzazione nella cache di file**<br>`FileCacheModule` | No | [Middleware di memorizzazione nella cache delle risposte](xref:performance/caching/middleware)
**La memorizzazione nella cache di HTTP**<br>`HttpCacheModule` | No | [Middleware di memorizzazione nella cache delle risposte](xref:performance/caching/middleware)
**Registrazione HTTP**<br>`HttpLoggingModule` | Yes | [Registrazione di ASP.NET Core](xref:fundamentals/logging/index)<br>Le implementazioni: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-extensions-logging)
**Reindirizzamento HTTP**<br>`HttpRedirectionModule` | Yes | [Middleware di riscrittura URL](xref:fundamentals/url-rewriting)
**Autenticazione Mapping certificati Client IIS**<br>`IISCertificateMappingAuthenticationModule` | Yes | 
**Restrizioni per IP e domini**<br>`IpRestrictionModule` | Yes | 
**Filtri ISAPI**<br>`IsapiFilterModule` | Yes | [Middleware](xref:fundamentals/middleware)
**ISAPI**<br>`IsapiModule` | Yes | [Middleware](xref:fundamentals/middleware)
**Supporto del protocollo**<br>`ProtocolSupportModule` | Yes | 
**Filtro richieste**<br>`RequestFilteringModule` | Yes | [Middleware di riscrittura URL`IRule`](xref:fundamentals/url-rewriting#irule-based-rule)
**Monitoraggio richieste**<br>`RequestMonitorModule` | Yes | 
**La riscrittura URL**<br>`RewriteModule` | Yes† | [Middleware di riscrittura URL](xref:fundamentals/url-rewriting)
**Server Side Includes**<br>`ServerSideIncludeModule` | No | 
**Compressione statica**<br>`StaticCompressionModule` | No | [Middleware di compressione delle risposte](xref:performance/response-compression)
**Contenuto statico**<br>`StaticFileModule` | No | [Middleware di File statici](xref:fundamentals/static-files)
**La memorizzazione nella cache token**<br>`TokenCacheModule` | Yes | 
**Memorizzazione nella cache URI**<br>`UriCacheModule` | Yes | 
**Autorizzazione URL**<br>`UrlAuthorizationModule` | Yes | [Identità di ASP.NET Core](xref:security/authentication/identity)
**Autenticazione di Windows**<br>`WindowsAuthenticationModule` | Yes | 

Del †the URL Rewrite Module `isFile` e `isDirectory` non funziona con le app ASP.NET Core a causa di modifiche in [struttura di directory](xref:host-and-deploy/directory-structure).

## <a name="managed-modules"></a>Moduli gestiti

Modulo | .NET core attivo | Opzione di ASP.NET Core
--- | :---: | ---
AnonymousIdentification | No | 
DefaultAuthentication | No | 
FileAuthorization | No | 
FormsAuthentication | No | [Cookie di Middleware di autenticazione](xref:security/authentication/cookie)
OutputCache | No | [Middleware di memorizzazione nella cache delle risposte](xref:performance/caching/middleware)
Profilo | No | 
RoleManager | No | 
ScriptModule-4.0 | No | 
Sessione | No | [Middleware di sessione](xref:fundamentals/app-state)
UrlAuthorization | No | 
UrlMappingsModule | No | [Middleware di riscrittura URL](xref:fundamentals/url-rewriting)
UrlRoutingModule-4.0 | No | [Identità di ASP.NET Core](xref:security/authentication/identity)
WindowsAuthentication | No | 

## <a name="iis-manager-application-changes"></a>Modifiche all'applicazione di gestione IIS

Utilizzando Gestione IIS per configurare le impostazioni, il *Web. config* file dell'app è stato modificato. Se la distribuzione di un'app e includono *Web. config*, eventuali modifiche apportate con Gestione IIS vengono sovrascritti da distribuito *Web. config* file. Se vengono apportate modifiche al server *Web. config* file, copiare l'aggiornamento *Web. config* file immediatamente nel progetto locale.

## <a name="disabling-iis-modules"></a>La disabilitazione di moduli IIS

Se un modulo IIS viene configurato a livello di server che deve essere disabilitato per un'app, un'aggiunta all'app *Web. config* file è possibile disabilitare il modulo. Lasciare il modulo sul posto e disattivarlo tramite un'impostazione di configurazione (se disponibile) o rimuovere il modulo dall'app.

### <a name="module-deactivation"></a>Disattivazione di modulo

Molti moduli offrono un'impostazione di configurazione che consenta di essere disattivati senza rimuoverli dall'app. Questo è il modo più semplice e rapido per disattivare un modulo. Ad esempio, se desiderano disattivare IIS URL Rewrite Module, utilizzare il `<httpRedirect>` elemento come illustrato di seguito. Per ulteriori informazioni sulla disabilitazione di moduli con le impostazioni di configurazione, seguire i collegamenti nel *gli elementi figlio* sezione [IIS `<system.webServer>` ](https://docs.microsoft.com/iis/configuration/system.webServer/).

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

### <a name="module-removal"></a>Rimozione del modulo

Se sceglie di rimuovere un modulo con un'impostazione in *Web. config*, sblocco del modulo e il `<modules>` sezione *Web. config* prima. Di seguito sono descritti i passaggi:

1. Sbloccare il modulo a livello di server. Fare clic sul server IIS in Gestione IIS **connessioni** barra laterale. Aprire il **moduli** nel **IIS** area. Fare clic sul modulo nell'elenco. Nel **azioni** barra laterale a destra, fare clic su **Unlock**. Sbloccare tutti moduli previsti da rimuovere dalla *Web. config* in un secondo momento.

2. Distribuire l'app senza un `<modules>` sezione *Web. config*. Se un'applicazione viene distribuita con un *Web. config* contenente il `<modules>` sezione senza avere sbloccato la sezione prima in Gestione IIS, Configuration Manager genera un'eccezione durante il tentativo di sbloccare la sezione. Pertanto, distribuire l'app senza un `<modules>` sezione.

3. Sbloccare il `<modules>` sezione *Web. config*. Nel **connessioni** barra laterale, selezionare il sito Web in **siti**. Nel **Management** area, aprire il **Editor di configurazione**. Utilizzare i controlli di spostamento selezionare il `system.webServer/modules` sezione. Nel **azioni** barra laterale a destra, fare clic su **Unlock** la sezione.

4. A questo punto, un `<modules>` sezione può essere aggiunti al *Web. config* file con un `<remove>` elemento da rimuovere il modulo dall'app. Più `<remove>` è possibile aggiungere elementi per rimuovere più moduli. Non dimenticare che se *Web. config* vengono apportate modifiche nel server per rendere immediatamente nel progetto in locale. Rimozione di un modulo in questo modo non influisce sull'utilizzo del modulo con altre applicazioni nel server.

  ```xml
  <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
  </configuration>
  ```

Per un'installazione di IIS con i moduli predefiniti installati, utilizzare la seguente `<module>` sezione per rimuovere i moduli predefiniti.

```xml
<modules>
  <remove name="CustomErrorModule" />
  <remove name="DefaultDocumentModule" />
  <remove name="DirectoryListingModule" />
  <remove name="HttpCacheModule" />
  <remove name="HttpLoggingModule" />
  <remove name="ProtocolSupportModule" />
  <remove name="RequestFilteringModule" />
  <remove name="StaticCompressionModule" /> 
  <remove name="StaticFileModule" /> 
</modules>
```

Un modulo IIS può essere rimosso anche con *Appcmd.exe*. Fornire il `MODULE_NAME` e `APPLICATION_NAME` nella riga di comando mostrata di seguito:

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

Di seguito viene illustrato come rimuovere il `DynamicCompressionModule` dal sito Web predefinito:

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a>Configurazione del modulo minimo

I solo i moduli necessari per eseguire un'applicazione ASP.NET di base sono il modulo di autenticazione anonima e il modulo di base di ASP.NET.

![Aprire Gestione IIS moduli con la configurazione minima modulo illustrata](modules/_static/modules.png)

## <a name="additional-resources"></a>Risorse aggiuntive

* [Host in Windows con IIS](xref:host-and-deploy/iis/index)
* [Panoramica di moduli IIS](https://docs.microsoft.com/iis/get-started/introduction-to-iis/iis-modules-overview)
* [Personalizzazione di IIS 7.0 ruoli e i moduli](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS `<system.webServer>`](https://docs.microsoft.com/iis/configuration/system.webServer/)