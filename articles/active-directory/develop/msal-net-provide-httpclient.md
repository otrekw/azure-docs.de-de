---
title: Bereitstellen von HttpClient und Proxy (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: Weitere Informationen zur Bereitstellung eines eigenen HttpClient und Proxys für die Verbindung mit Azure AD über die Microsoft Authentication Library für .NET (MSAL.NET).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 04/23/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: devx-track-csharp, aaddev
ms.openlocfilehash: 6099bfe339ae398e4683f17a4397656a4507b6ef
ms.sourcegitcommit: c28fc1ec7d90f7e8b2e8775f5a250dd14a1622a6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2020
ms.locfileid: "88166007"
---
# <a name="providing-your-own-httpclient-and-proxy-using-msalnet"></a>Bereitstellen eines eigenen HttpClient und Proxys über MSAL.NET
Bei der [Initialisierung einer öffentlichen Clientanwendung](msal-net-initializing-client-applications.md) können Sie mit `.WithHttpClientFactory method` einen eigenen HttpClient bereitstellen.  Die Bereitstellung eines eigenen HttpClient ermöglicht fortgeschrittene Szenarien wie die differenzierte Steuerung eines HTTP-Proxys, das Anpassen von Benutzer-Agent-Headern oder das Erzwingen von MSAL, einen bestimmten HttpClient zu verwenden (z. B. in ASP.NET Core-Web-Apps/-APIs).

## <a name="initialize-with-httpclientfactory"></a>Initialisieren mit HttpClientFactory
Das folgende Beispiel zeigt, wie Sie ein `HttpClientFactory` erstellen und dann damit eine öffentliche Clientanwendung initialisieren können:

```csharp
IMsalHttpClientFactory httpClientFactory = new MyHttpClientFactory();

var pca = PublicClientApplicationBuilder.Create(MsalTestConstants.ClientId) 
                                        .WithHttpClientFactory(httpClientFactory)
                                        .Build();
```

## <a name="httpclient-and-xamarin-ios"></a>HttpClient und Xamarin iOS
Bei der Verwendung von Xamarin iOS wird empfohlen, ein `HttpClient` zu erstellen, das explizit den `NSURLSession`-basierten Handler für iOS 7 und höher verwendet. MSAL.NET erstellt automatisch ein `HttpClient`, das `NSURLSessionHandler` für iOS 7 und höher verwendet. Weitere Informationen finden Sie in der [Xamarin iOS-Dokumentation für HttpClient](/xamarin/cross-platform/macios/http-stack).