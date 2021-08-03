---
title: Bereitstellen von HttpClient und Proxy (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie mithilfe der Microsoft-Authentifizierungsbibliothek für .NET (Microsoft Authentication Library for .NET, MSAL.NET) selbst einen HttpClient und Proxy für die Verbindung mit Azure AD bereitstellen.
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
ms.openlocfilehash: 4546e1531b33f2717c544a76d477f5e7d77c88ad
ms.sourcegitcommit: df574710c692ba21b0467e3efeff9415d336a7e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110664271"
---
# <a name="providing-your-own-httpclient-and-proxy-using-msalnet"></a>Bereitstellen eines eigenen HttpClient und Proxys über MSAL.NET
Bei der [Initialisierung einer Clientanwendung](msal-net-initializing-client-applications.md) können Sie mit `.WithHttpClientFactory method` einen eigenen HttpClient bereitstellen.  Die Bereitstellung eines eigenen HttpClient ermöglicht fortgeschrittene Szenarien wie die differenzierte Steuerung eines HTTP-Proxys, das Anpassen von Benutzer-Agent-Headern oder das Erzwingen von MSAL, einen bestimmten HttpClient zu verwenden (z. B. in ASP.NET Core-Web-Apps/-APIs).

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
