---
title: Löschen des Tokencaches mit MSAL.NET | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie den Tokencache mit der Microsoft Authentication Library für .NET (MSAL.NET) löschen.
services: active-directory
author: TylerMSFT
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f203bc057482466f6bddc7523c0ec7a7e9404ccc
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2019
ms.locfileid: "74915922"
---
# <a name="clear-the-token-cache-using-msalnet"></a>Löschen des Tokencaches mit MSAL.NET

Wenn Sie mit der Microsoft Authentication Library für .NET (MSAL.NET) [ein Zugriffstoken abrufen](msal-acquire-cache-tokens.md), wird das Token zwischengespeichert. Wenn die Anwendung ein Token benötigt, sollte sie zuerst die `AcquireTokenSilent`-Methode aufrufen, um zu überprüfen, ob ein akzeptables Token im Cache vorhanden ist. 

Zum Löschen des Caches werden die Konten aus dem Cache entfernt. Die Sitzungscookies, die sich im Browser befinden, werden jedoch nicht entfernt.  Im folgenden Beispiel werden eine öffentliche Clientanwendung instanziiert, die Konten für die Anwendung abgerufen und die Konten entfernt.

```csharp
private readonly IPublicClientApplication _app;
private static readonly string ClientId = ConfigurationManager.AppSettings["ida:ClientId"];
private static readonly string Authority = string.Format(CultureInfo.InvariantCulture, AadInstance, Tenant);

_app = PublicClientApplicationBuilder.Create(ClientId)
                .WithAuthority(Authority)
                .Build();

var accounts = (await _app.GetAccountsAsync()).ToList();

// clear the cache
while (accounts.Any())
{
   await _app.RemoveAsync(accounts.First());
   accounts = (await _app.GetAccountsAsync()).ToList();
}

```

Weitere Informationen zum Abrufen und Zwischenspeichern von Token finden Sie unter [Abrufen und Zwischenspeichern von Token mit MSAL](msal-acquire-cache-tokens.md).
