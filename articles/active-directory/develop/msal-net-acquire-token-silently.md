---
title: Abrufen eines Token aus dem Cache (MSAL.NET)
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie ein Zugriffstoken mithilfe von Microsoft Authentication Library für .NET (MSAL.NET) automatisch (aus dem Tokencache) abgerufen werden kann.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 07/16/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: devx-track-csharp, aaddev
ms.openlocfilehash: 95cfb937ce236d0acd1a3369068afc6f3b505aed
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88166245"
---
# <a name="get-a-token-from-the-token-cache-using-msalnet"></a>Abrufen eines Tokens aus dem Tokencache mithilfe von MSAL.NET

Wenn Sie ein Zugriffstoken mithilfe von Microsoft Authentication Library für .NET (MSAL.NET) abrufen, wird das Token zwischengespeichert. Wenn die Anwendung ein Token benötigt, sollte sie zuerst die `AcquireTokenSilent`-Methode aufrufen, um zu überprüfen, ob ein akzeptables Token im Cache vorhanden ist. In vielen Fällen ist es möglich, anhand eines Tokens im Cache ein weiteres Token mit zusätzlichen Bereichen abzurufen. Es ist auch möglich, ein Token zu aktualisieren, wenn es bald abläuft (da der Tokencache auch ein Aktualisierungstoken enthält).

Das empfohlene Muster besteht darin, zuerst die `AcquireTokenSilent`-Methode aufzurufen.  Wenn `AcquireTokenSilent` fehlschlägt, wird dann ein Token mit anderen Methoden abgerufen.

Im folgenden Beispiel versucht die Anwendung zunächst, ein Token aus dem Tokencache abzurufen.  Wenn eine `MsalUiRequiredException` -Ausnahme ausgelöst wird, ruft die Anwendung interaktiv ein Token ab. 

```csharp
AuthenticationResult result = null;
var accounts = await app.GetAccountsAsync();

try
{
 result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
        .ExecuteAsync();
}
catch (MsalUiRequiredException ex)
{
 // A MsalUiRequiredException happened on AcquireTokenSilent.
 // This indicates you need to call AcquireTokenInteractive to acquire a token
 System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

 try
 {
    result = await app.AcquireTokenInteractive(scopes)
          .ExecuteAsync();
 }
 catch (MsalException msalex)
 {
    ResultText.Text = $"Error Acquiring Token:{System.Environment.NewLine}{msalex}";
 }
}
catch (Exception ex)
{
 ResultText.Text = $"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}";
 return;
}

if (result != null)
{
 string accessToken = result.AccessToken;
 // Use the token
}
```
